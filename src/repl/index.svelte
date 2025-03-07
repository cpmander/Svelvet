<script lang="ts">
  import { setContext, createEventDispatcher } from 'svelte';
  import { get, writable } from 'svelte/store';
  import SplitPane from './SplitPane.svelte';
  import InputOutputToggle from './InputOutputToggle.svelte';
  import ComponentSelector from './Input/ComponentSelector.svelte';
  import ModuleEditor from './Input/ModuleEditor.svelte';
  import Output from './Output/index.svelte';
  import Bundler from './Bundler.js';
  import { is_browser } from './env.js';
  import { userInfoStore } from '../authStoreTs';
  import saveIcon from '../assets/DB save icon.svg';
	import copyIcon from '../assets/DB copy icon.svg';
	import deleteIcon from '../assets/DB delete icon.svg';

  export let packagesUrl = 'https://unpkg.com';
  export let svelteUrl = `${packagesUrl}/svelte`;
  export let embedded = false;
  export let orientation = 'columns';
  export let relaxed = false;
  export let fixed = false;
  export let fixedPos = 50;
  export let injectedJS = '';
  export let injectedCSS = '';
  export let theme = 'svelte';
  export let showModified = false;
  export let showAst = false;

  let { user, diagrams } = userInfoStore; 

const historyMap = new Map();

export function toJSON() {
  return {
    imports: $bundle.imports,
    components: $components
  };
}

export async function set(data) {
  components.set(data.components);
  selected.set(data.components[0]);

  rebundle();

  await module_editor_ready;
  await output_ready;

  injectedCSS = data.css || '';
  await module_editor.set($selected.source, $selected.type);
  output.set($selected, $compile_options);

  historyMap.clear();
  module_editor.clearHistory();
}

export function markSaved() {
  components.update((components) =>
    components.map((c) => {
      c.modified = false;
      return c;
    })
  );
  selected.update((c) => c);
}

export function update(data) {
  const { name, type } = $selected || {};

  components.set(data.components);

  const matched_component = data.components.find(
    (file) => file.name === name && file.type === type
  );
  selected.set(matched_component || data.components[0]);

  injectedCSS = data.css || '';

  if (matched_component) {
    module_editor.update(matched_component.source);
    output.update(matched_component, $compile_options);
  } else {
    module_editor.set(matched_component.source, matched_component.type);
    output.set(matched_component, $compile_options);

    module_editor.clearHistory();
  }
}

const dispatch = createEventDispatcher();

const components = writable([]);
const selected = writable(null);
const bundle = writable(null);
const cursor_index = writable(0);
const toggleable = writable(false);

const compile_options = writable({
  generate: 'dom',
  dev: false,
  css: false,
  hydratable: false,
  customElement: false,
  immutable: false,
  legacy: false
});

let module_editor: { set: (arg0: any,arg1: any) => void; clearHistory: () => void; update: (arg0: any) => void; focus: () => void; setCursor: (arg0: { line: number; ch: any; }) => void; cursorIndex: { subscribe: (arg0: (index: any) => void) => void; }; unmarkText: () => void; markText: (arg0: { from: any; to: any; }) => void; getHistory: () => any; setHistory: (arg0: any) => void; loadSavedCode: (arg0: any) => void; getCodeEditorValue: (arg0: any,arg1: any) => void; deleteCode: (arg0: any) => void; copyCodeEditor: () => void; };
let output;

let width = 0;
let show_output = false;

let current_token;
async function rebundle() {
  const token = (current_token = {});
  const result = await bundler.bundle($components);
  if (result && token === current_token) bundle.set(result);
}

// TODO this is a horrible kludge, written in a panic. fix it
let fulfil_module_editor_ready;
let module_editor_ready = new Promise((f) => (fulfil_module_editor_ready = f));

let fulfil_output_ready;
let output_ready = new Promise((f) => (fulfil_output_ready = f));

setContext('REPL', {
  components,
  selected,
  bundle,
  compile_options,

  cursor_index,
  toggleable,
  module_editor_ready,

  rebundle,

  navigate: (item) => {
    const match = /^(.+)\.(\w+)$/.exec(item.filename);
    if (!match) return; // ???

    const [, name, type] = match;
    const component = $components.find((c) => c.name === name && c.type === type);
    handle_select(component);
/* I think this is what's breaking our REPL */
    setTimeout(() => {
      module_editor.focus();
      module_editor.setCursor({
        line: item.start.line - 1,
        ch: item.start.column
      });
    }, 0);
  },

  handle_change: (event) => {
    selected.update((component) => {
      // TODO this is a bit hacky — we're relying on mutability
      // so that updating components works... might be better
      // if a) components had unique IDs, b) we tracked selected
      // *index* rather than component, and c) `selected` was
      // derived from `components` and `index`
      if (component.source != event.detail.value) {
        component.source = event.detail.value;
        component.modified = true;
      }
      return component;
    });

    components.update((component) => {
      if (component.name === $selected.name) {
        return $selected;
      }

      return component;
    });

    // recompile selected component
    output.update($selected, $compile_options);

    rebundle();

    dispatch('change', {
      components: $components
    });
  },

  register_module_editor(editor) {
    module_editor = editor;
    module_editor.cursorIndex.subscribe((index) => {
      cursor_index.set(index);
    });
    fulfil_module_editor_ready();
  },

  register_output(handlers) {
    output = handlers;
    fulfil_output_ready();
  },

  request_focus() {
    module_editor.focus();
  },

  mark_text({ from, to }) {
    module_editor.unmarkText();
    module_editor.markText({ from, to });
  },

  unmark_text() {
    module_editor.unmarkText();
  }
});

function handle_select(component) {
  historyMap.set(get_component_name($selected), module_editor.getHistory());
  selected.set(component);
  module_editor.set(component.source, component.type);
  if (historyMap.has(get_component_name($selected))) {
    module_editor.setHistory(historyMap.get(get_component_name($selected)));
  } else {
    module_editor.clearHistory();
  }
  output.set($selected, $compile_options);
}

function get_component_name(component) {
  return `${component.name}.${component.type}`;
}

function beforeUnload(event) {
  if (showModified && $components.find((component) => component.modified)) {
    event.preventDefault();
    event.returnValue = '';
  }
}

let sourceErrorLoc;

let status = null;
let status_visible = false;
let status_timeout = null;

const bundler =
  is_browser &&
  new Bundler({
    packagesUrl,
    svelteUrl,
    onstatus: (message) => {
      if (message) {
        // show bundler status, but only after time has elapsed, to
        // prevent the banner flickering
        if (!status_visible && !status_timeout) {
          status_timeout = setTimeout(() => {
            status_visible = true;
          }, 400);
        }
      } else {
        clearTimeout(status_timeout);
        status_visible = false;
        status_timeout = null;
      }

      status = message;
    }
  });

$: if (output && $selected) {
  output.update($selected, $compile_options);
}

$: mobile = width < 540;

$: $toggleable = mobile && orientation === 'columns';

// ----- NEW FUNCTION AS OF SVELVET 2.0 -----


let diagramSelected: any;

const loadSavedDiagram = (): void => {
  module_editor.loadSavedCode(diagramSelected.code);
  document.getElementById('project-name').value = diagramSelected.diagram_name;
};

const saveDiagram = (): void => {
  if (user) {
    module_editor.getCodeEditorValue(
      diagramSelected.id,
      document?.getElementById('project-name').value
    );
  } else {
    alert('You must sign in before attempting to save a diagram.');
  }
};

const deleteDiagram = (): void => {
  if (user && diagramSelected.id) {
    module_editor.deleteCode(diagramSelected.id);
  } else if (user) {
    alert('You must name and save the diagram before attempting to delete the diagram.');
  } else {
    alert('You must sign in and save the diagram before attempting to delete the diagram.');
  }
};

const copyCodeToClipboard = async (): void => {
  await module_editor.copyCodeEditor();
  alert('Code copied to clipboard!');
};
</script>


<svelte:window on:beforeunload={beforeUnload} />

<!-- NEW DROPDOWN MENU TO SELECT SAVED DIAGRAMS AS OF SVELVET 2.0 -->
{#if $user}

<div class="repl-navbar">
  <input id="project-name" class="display:inline-block bg-gray-50 border border-gray-300 text-gray-900 text-lg rounded-lg focus:ring-blue-500 focus:border-blue-500 p-2.5" placeholder="Save As..." style="min-width:275px" required/>

  <div class="icons-navbar">
    <!-- added a save button to limit user to 5 projects -->
    <button class="db-icons" on:click={saveDiagram}>
      Save<img class="db-icons" src={saveIcon} alt="save-icon" />
    </button>
    <!-- added a button to delete projects in case the users have more than 5 -->
    <button class="db-icons" on:click={deleteDiagram}>
      Delete<img class="db-icons" src={deleteIcon} alt="delete-icon" />
    </button>
    <!-- added a button to allow users to update previously saved projects and reflect changes in db -->
    <button class="db-icons" on:click={copyCodeToClipboard}>
      Copy<img class="db-icons" src={copyIcon} alt="copy-icon" />
    </button>
  </div>
    <select id="selectElement" class="bg-gray-50 border border-gray-300 text-gray-900 text-lg rounded-lg focus:ring-blue-500 focus:border-blue-500 p-2.5" bind:value={diagramSelected} on:change={loadSavedDiagram} required>
    <option value="" disabled selected>Previously Saved Projects</option>
    {#each $diagrams as diagram}
    <option value={diagram}>
      Name: {diagram.diagram_name}, Created at: {diagram.created_at.slice(0, diagram.created_at.indexOf('T'))}
    </option>
    {/each}
  </select>
</div>

{/if}
{#if !$user}
<div class='repl-navbar'>
  Please log in to save your diagrams.
  <button class="db-icons" on:click={copyCodeToClipboard}>
    Copy<img class="db-icons" src={copyIcon} alt="copy-icon" />
  </button>
</div>
{/if}

<!-- NEW BUTTONS AS OF SVELVET 2.0 -->


<!-- REPL ELEMENTS MADE BY SVELTE DEVS -->
<div class="container" class:toggleable={$toggleable} bind:clientWidth={width}>
<div class="viewport" class:output={show_output}>
  <SplitPane
    type={orientation === 'rows' ? 'vertical' : 'horizontal'}
    pos={mobile || fixed ? fixedPos : orientation === 'rows' ? 50 : 60}
    {fixed}
  >
    <section slot="a">
      <ComponentSelector show_modified={showModified} {handle_select} on:add on:remove />
      <ModuleEditor errorLoc={sourceErrorLoc} {theme} />
    </section>

    <section slot="b" style="height: 100%;">
      <Output
        {svelteUrl}
        status={status_visible && status}
        {embedded}
        {relaxed}
        {injectedJS}
        {injectedCSS}
        {theme}
        {showAst}
      />
    </section>
  </SplitPane>
</div>
{#if $toggleable}
  <InputOutputToggle bind:checked={show_output} />
{/if}
</div>

<style>

/* STYLING FOR MOBILE */
@media (max-width: 540px) {
  .repl-navbar-noUser {
  display: flex;
  align-items: center;
  padding: 0.25em;
}
  .repl-navbar {
    padding: 0.25em;
    display: flex;
    flex-direction: column-reverse;
    align-items: center;
    margin-top: 1em;
  }

  #project-name {
    display: inline-block;
    height: 4em;
    width: 22em;
    font-size: .8em;
  }

  #selectElement {
    height: 4em;
    font-size: .8em;
    width: 22em;
    justify-self: center;
  }
  
  .db-icons {
  display: inline-flex;
  align-items: center;
  width: 3em;
  height: 3em;
  padding: 0.15em;
}

.db-icons img {
    display: inline-block;
    padding: 0em 0em 1.75em;
  }

  button {
  display: flex;
  flex-direction: column;
  background-color: rgb(241, 241, 241);
  margin: 1em 1em 1em;
  box-shadow: 0 9px rgb(228, 224, 224);
  border-radius: 10px;
  color: rgb(244 63 94);
}

  button:hover {background-color: rgb(215, 215, 215)}

  button:active {
  background-color: rgb(33, 250, 0);
  box-shadow: 0 9px rgb(10, 73, 1);
  transform: translateY(4px);
}

}
/* STYLING FOR MOBILE */

@media (min-width: 540px) {

  .repl-navbar-noUser {
    display: flex;
    justify-content: flex-start;
    min-width: 500px;
    padding: 0.5em;
  }

  .repl-navbar {
    display: flex;
    flex-direction: row;
    justify-content: flex-start;
    padding: 0.5em;
    min-width: 1350px;
  }

  .db-icons {
    display: inline-block;
    width: 4rem;
    height: 4rem;
  }
  .db-icons img {
    display: inline-block;
    padding: 0em 0em 1.75em;
  }
/* changed button display prop to flex and direction to column */
  button {
  display: flex;
  flex-direction: column;

  background-color: rgb(241, 241, 241);
  margin: 0em 3.5em 0em;
  box-shadow: 0 9px rgb(228, 224, 224);
  border-radius: 7px;
  color: rgb(244 63 94);
}
button:hover {background-color: rgb(215, 215, 215)}

button:active {
  background-color: rgb(33, 250, 0);
  box-shadow: 0 5px rgb(10, 73, 1);
  transform: translateY(4px);
}
}
/* 	button {
  background-color: rgb(244 63 94);
  border: none;
  color: white;
  padding: 2px 2px;

  text-align: center;
  text-decoration: none;

  font-size: 12px;
} */
.container {
  position: relative;
  width: 100%;
  /* this is what I fixed to have the repl fit in the screen */
  height: 90%;
  background: white;
}

.container :global(section) {
  position: relative;
  /* changed 42-0-0-0 to 0-0-0-0 */
  padding: 0 0 0 0;
  height: 100%;
  box-sizing: border-box;
}

.container :global(section) > :global(*):first-child {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  /* changed 42 - 0 */
  height: 0;
  box-sizing: border-box;
}

.container :global(section) > :global(*):last-child {
  width: 100%;
  height: 100%;
}

.viewport {
  height: 100%;
}

.toggleable .viewport {
  width: 200%;
  height: calc(100% - 42px);
  transition: transform 0.3s;
}

.toggleable .viewport.output {
  transform: translate(-50%);
}

select:invalid {
  color: gray;
}

.container :global(section) {
  position: relative;
  /* changed */
  padding: 42px 0 0 0;
  height: 100%;
  box-sizing: border-box;
}

.container :global(section) > :global(*):first-child {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 42px;
  box-sizing: border-box;
}

.container :global(section) > :global(*):last-child {
  width: 100%;
  height: 100%;
}

.viewport {
  height: 100%;
}

.toggleable .viewport {
  width: 200%;
  height: calc(100% - 42px);
  transition: transform 0.3s;
}

.toggleable .viewport.output {
  transform: translate(-50%);
}

select:invalid {
  color: gray;
}
</style>