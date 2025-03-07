<script lang="ts">
  import linkedin from '../assets/linkedin-svgrepo-com.svg';
  import github from '../assets/github-svgrepo-com.svg';
  import anu from '../assets/profiles/Anu-small.png';
  import andrew from '../assets/profiles/Andrew-small.png';
  import alexander from '../assets/profiles/Alexander-small.png';
  import aaron from '../assets/profiles/Aaron-small.png';
  import justin from '../assets/profiles/Justin-small.png';
  import damian from '../assets/profiles/Damian-small.png';
  import von from '../assets/profiles/Von-small.png';
  import walter from '../assets/profiles/Walter-small.png';
  import ali from '../assets/profiles/Ali-small.png';

  import Svelvet from '$lib/Containers/Svelvet/index.svelte';
  // import type { Node, Edge } from '$lib/types/types';
  import type { Node, Edge } from 'svelvet';
  //import Svelvet from 'svelvet';

  const initialNodes: Node[] = [
    {
      id: 1,
      position: { x: 225, y: 10 },
      data: { label: 'Add Images!' },
      width: 100,
      height: 100,
      bgColor: 'white',
      borderColor: 'transparent',
      image: true,
      src: 'https://svelvet.io/_app/assets/Logo%201-cc7b0baf.svg'
    },
    {
      id: 2,
      position: { x: 390, y: 180 },
      data: { label: 'Mixed Anchors' },
      width: 125,
      height: 40,
      bgColor: 'white',
      targetPosition: 'left'
    },
    {
      id: 3,
      position: { x: 225, y: 260 },
      data: { label: 'Output Node' },
      width: 100,
      height: 40,
      bgColor: '#FFE4E6'
    },
    {
      id: 4,
      position: { x: 25, y: 180 },
      data: { label: 'Drag me!' },
      width: 125,
      height: 40,
      bgColor: 'white',
      targetPosition: 'right'
    },
    {
      id: 5,
      position: { x: 390, y: 380 },
      data: { label: 'Custom Node' },
      width: 125,
      height: 40,
      bgColor: '#C8FFC7',
      borderColor: 'transparent',
      borderRadius: 0
    },
    {
      id: 6,
      position: { x: 47.5, y: 360 },
      data: { label: 'Custom Node' },
      width: 80,
      height: 80,
      borderColor: '#FF4121',
      borderRadius: 30,
      bgColor: 'white',
      textColor: '#FF4121'
      //targetPosition: 'right'
    }
  ];

  const initialEdges: Edge[] = [
    { id: 'e1-2', source: 1, target: 2, label: 'edge label' },
    { id: 'e2-3', source: 2, target: 3, animate: true, label: 'animated edges' },
    { id: 'e1-4', source: 1, target: 4, type: 'step', animate: true, edgeColor: '#FF4121' },
    {
      id: 'e2-5',
      source: 2,
      target: 5,
      label: 'colored edges',
      animate: true,
      arrow: true,
      edgeColor: '#FF4121',
      labelBgColor: '#1F2937',
      labelTextColor: '#FFE4E6'
    },
    { id: 'e2-5', source: 4, target: 6, type: 'straight' },
    {
      id: 'e2-5',
      source: 3,
      target: 6,
      type: 'smoothstep',
      label: 'colored label',
      labelBgColor: '#FF4561',
      labelTextColor: 'white',
      animate: true
    }
  ];
</script>

<svelte:head>
  <title>Svelvet</title>
</svelte:head>
<div class="css-blurry-gradient" />

<div class="flex flex-col justify-center items-center w-auto my-24 md:my-48 mx-8 text-center ">
  <h1 class="text-4xl md:text-6xl text-center font-bold text-gray-800 max-w-screen-lg">
    Mapping out your ideas with Svelte has never been easier
  </h1>
  <p class="text-lg md:mx-16 my-8 text-gray-500">
    <code class="bg-rose-100 rounded-lg p-1">Svelvet</code> is a lightweight Svelte component library
    for building interactive node-based flow diagrams
  </p>
  <a href="/docs/installation" class="px-6 py-3 btn-primary">Get Started</a>
</div>
<div
  class="h-[700px] md:h-[600px] w-screen bg-gradient-to-r from-[#F66F52] via-[#f86f6f] to-rose-400 text-white shadow-xl"
>
  <div class="flex flex-col md:flex-row h-full m-12 md:m-24 md:gap-10">
    <div class="w-full md:w-1/2 h-full flex flex-col justify-center">
      <h3 class="text-4xl font-bold my-4 text-center md:text-right">Beautiful and Customizable</h3>
      <p class=" text-gray-100 text-center md:text-right">
        Svelvet allows you to easily create intuitive flow diagrams using pre-built components with
        seamless zooming and panning, draggable interactivity, customizable edges and nodes and
        more!
      </p>
      <a
        href="/docs/core-concepts"
        class="hidden shadow-md hover:shadow-transparent md:flex self-end w-fit border-2 border-transparent hover:border-white mt-5 py-2 px-5 rounded-full hover:ease-in duration-100 hover:text-white hover:bg-transparent bg-white text-gray-800 "
        >Learn More</a
      >
    </div>
    <div
      class="w-full md:w-1/2 h-[350px] md:h-full mb-16 md:py-12 flex flex-col justify-center text-center"
    >
      <div
        class="bg-white max-w-full md:max-w-[550px] h-full relative overflow-hidden border rounded-xl shadow-md self-center md:self-auto"
      >
        <Svelvet nodes={initialNodes} edges={initialEdges} width={550} height={550} background />
      </div>
    </div>
  </div>
</div>

<div class="h-fit md:ml-8 md:mr-8 md:w-fit">
  <div class="flex flex-col-reverse items-center md:flex-row h-full mx-12 my-20 md:my-32 md:gap-10">
    <div
      class="w-full md:w-1/2 my-4 md:h-full md:py-12 flex flex-col justify-center items-center text-center "
    >
      <div
        class="overflow-hidden rounded-lg bg-gray-100 w-[320px] md:w-[380px] grid place-items-center wrapper"
      >
        <div class="h-7 w-full bg-gray-300 flex items-center">
          <div class="btn bg-[#FF605C]" />
          <div class="btn bg-[#FFBD44]" />
          <div class="btn bg-[#00CA4E]" />
        </div>
        <div class="typing-demo my-3">npm install svelvet</div>
      </div>
    </div>
    <div class="sm:mt-0 w-full md:w-1/2 h-full flex flex-col justify-center">
      <h3 class="text-4xl font-bold my-4 text-center md:text-left text-gray-700">
        Get Started Easily
      </h3>
      <p class="mb-4 text-gray-500 text-center md:text-left">
        Start mapping out your ideas with our NPM package. Simply provide your node data to the
        <code class="bg-rose-100 rounded-lg p-1">Svelvet</code> component and you're ready to show off
        your interactive diagram - no extra configuration needed!
      </p>
      <a href="/docs/installation" class="hidden md:flex mt-5 py-2 px-5 btn-primary">Install Now</a>
    </div>
  </div>
</div>
<div
  class="h-[1200px] contributors:h-1100px] w-full bg-gradient-to-r from-[#F66F52] via-[#f86f6f] to-rose-400 text-white shadow-xl"
>
  <div class=" h-full m-12 contributors:m-24">
    <div class=" h-full w-full p-1 flex flex-col justify-around">
      <h3 class="text-4xl font-bold mb-4 text-center">Contributors</h3>

      <!-- START TESTING CARDS -->
      <!-- <div class="flex flex-col contributors:flex-row my-4 items-center"> -->
      <div class="flex flex-col justify-around contributors:flex-row my-1 items-center">
        <div class="flex justify-evenly w-3/4 gap-2 m-1 contributors:gap-0">
          <!-- <div class="flex justify-around w-1/2 m-2 gap-2 contributors:gap-0"> -->
          <!-- CARD 1 -->
          <div class="contributor-card ">
            <img src={anu} alt="" class="hidden contributors:block h-32 rounded-full" />
            <p class="mt-3 mb-1 text-lg leading-6">Anu <br />Sharma</p>
            <p class="mb-3 text-[11px] text-gray-500">CO-CREATOR</p>
            <div class="flex gap-3 ml-3">
              <a target="_blank" href="https://www.linkedin.com/in/anu-sharma-6936a686/"
                ><img src={linkedin} alt="LinkedIn Icon" /></a
              >
              <a target="_blank" href="https://github.com/anulepau"
                ><img src={github} alt="GitHub Icon" /></a
              >
            </div>
          </div>
          <!-- CARD 2 -->
          <div class="contributor-card">
            <img src={andrew} alt="" class="hidden contributors:block h-32 rounded-full" />
            <p class="mt-3 mb-1 text-lg leading-6">Andrew <br />Widjaja</p>
            <p class="mb-3 text-[11px] text-gray-500">CO-CREATOR</p>
            <div class="flex gap-3 ml-3">
              <a target="_blank" href="https://www.linkedin.com/in/andrew-widjaja/"
                ><img src={linkedin} alt="LinkedIn Icon" /></a
              >
              <a target="_blank" href="https://github.com/andrew-widjaja"
                ><img src={github} alt="GitHub Icon" /></a
              >
            </div>
          </div>

          <!-- CARD 3 -->
          <div class="contributor-card">
            <img src={aaron} alt="" class="hidden contributors:block h-32 rounded-full" />
            <p class="mt-3 mb-1 text-lg leading-6">Aaron <br />Willett</p>
            <p class="mb-3 text-[11px] text-gray-500">CO-CREATOR</p>
            <div class="flex gap-3 ml-3">
              <a target="_blank" href="https://www.linkedin.com/in/awillettnyc/"
                ><img src={linkedin} alt="LinkedIn Icon" /></a
              >
              <a target="_blank" href="https://github.com/awillettnyc"
                ><img src={github} alt="GitHub Icon" /></a
              >
            </div>
          </div>
        </div>
      </div>
      <!-- <div class="flex flex-col justify-center contributors:flex-row my-4 items-center"> -->
      <div class="flex flex-col justify-around contributors:flex-row my-1 items-center">
        <!-- <div class="flex justify-evenly w-1/3 gap-2 contributors:gap-4"> -->
          <div class="flex justify-evenly w-3/4 gap-2 m-1 contributors:gap-0">
          <!-- CARD 1 -->
          <div class="contributor-card">
            <img src={alexander} alt="" class="hidden contributors:block h-32 rounded-full" />
            <p class="mt-3 mb-1 text-md leading-6">Alexander<br />Zambrano</p>
            <p class="mb-3 text-[11px] text-gray-500">CO-CREATOR</p>
            <div class="flex gap-3 ml-3">
              <a target="_blank" href="https://www.linkedin.com/in/alexander-z-8b7716b0/"
                ><img src={linkedin} alt="LinkedIn Icon" /></a
              >
              <a target="_blank" href="https://github.com/azambran21"
                ><img src={github} alt="GitHub Icon" /></a
              >
            </div>
          </div>
          <!-- CARD 2 -->
          <div class="contributor-card">
            <img src={justin} alt="" class="hidden contributors:block h-32 rounded-full" />
            <p class="mt-3 mb-1 text-lg leading-6">Justin <br />Wouters</p>
            <p class="mb-3 text-[11px] text-gray-500">CO-CREATOR</p>
            <div class="flex gap-3 ml-3">
              <a target="_blank" href="https://www.linkedin.com/in/justinwouters/"
                ><img src={linkedin} alt="LinkedIn Icon" /></a
              >
              <a target="_blank" href="https://github.com/justinwouters"
                ><img src={github} alt="GitHub Icon" /></a
              >
            </div>
          </div>
          <!-- CARD 3 -->
          <div class="contributor-card">
            <img src={damian} alt="" class="hidden contributors:block h-32 rounded-full" />
            <p class="mt-3 mb-1 text-lg leading-6">Damian <br />Lim</p>
            <p class="mb-3 text-[11px] text-gray-500">CO-CREATOR</p>
            <div class="flex gap-3 ml-3">
              <a target="_blank" href="https://www.linkedin.com/in/lim-damian/"
                ><img src={linkedin} alt="LinkedIn Icon" /></a
              >
              <a target="_blank" href="https://github.com/limd96"
                ><img src={github} alt="GitHub Icon" /></a
              >
            </div>
          </div>
        </div>
      </div>
      <div class="flex flex-col justify-around contributors:flex-row my-1 items-center">
        <div class="flex justify-evenly w-3/4 gap-2 m-1 contributors:gap-0">
          <!-- CARD 1 -->
          <div class="contributor-card">
            <img src={walter} alt="" class="hidden contributors:block h-32 rounded-full" />
            <p class="mt-3 mb-1 text-lg leading-6">Walter <br />DeVault</p>
            <p class="mb-3 text-[11px] text-gray-500">CO-CREATOR</p>
            <div class="flex gap-3 ml-3">
              <a target="_blank" href="https://www.linkedin.com/in/walter-devault/"
                ><img src={linkedin} alt="LinkedIn Icon" /></a
              >
              <a target="_blank" href="https://github.com/TensionCoding"
                ><img src={github} alt="GitHub Icon" /></a
              >
            </div>
          </div>
          <!-- CARD 2 -->
          <div class="contributor-card">
            <img src={von} alt="" class="hidden contributors:block h-32 rounded-full" />
            <p class="mt-3 mb-1 text-lg leading-6">Von <br />Garcia</p>
            <p class="mb-3 text-[11px] text-gray-500">CO-CREATOR</p>
            <div class="flex gap-3 ml-3">
              <a target="_blank" href="https://www.linkedin.com/in/gerard-von-g-3964bb160/"
                ><img src={linkedin} alt="LinkedIn Icon" /></a
              >
              <a target="_blank" href="https://github.com/vongarcia97"
                ><img src={github} alt="GitHub Icon" /></a
              >
            </div>
          </div>
          <!-- CARD 3 -->
          <div class="contributor-card">
            <img src={ali} alt="" class="hidden contributors:block h-32 rounded-full" />
            <p class="mt-3 mb-1 text-lg leading-6">Ali <br />Adams</p>
            <p class="mb-3 text-[11px] text-gray-500">CO-CREATOR</p>
            <div class="flex gap-3 ml-3">
              <a target="_blank" href="https://www.linkedin.com/in/alimadams/"
                ><img src={linkedin} alt="LinkedIn Icon" /></a
              >
              <a target="_blank" href="https://github.com/AliA12336"
                ><img src={github} alt="GitHub Icon" /></a
              >
            </div>
          </div>
        </div>
      </div>
      <p class=" text-gray-100 text-center mt-5 md:mb-0">
        Svelvet is open source and has room for many more features. We welcome contributors!
      </p>
      <a
        href="https://github.com/oslabs-beta/Svelvet"
        target="_blank"
        class="self-center w-fit border-2 border-transparent hover:border-white mt-4 py-2 px-5 rounded-full hover:ease-in duration-100 hover:text-white hover:bg-transparent bg-white text-gray-800 "
        >See Svelvet on GitHub</a
      >
    </div>
  </div>
</div>

<!-- END TESTING CARDS -->

<!-- START ORIGINAL CARDS -->
<!-- <div class="flex flex-col contributors:flex-row my-4 items-center">
        <div class="flex justify-evenly w-1/2 m-2 gap-2 contributors:gap-0">
          <div class="contributor-card">
            <img src={anu} alt="" class="hidden contributors:block h-28 rounded-full" />
            <p class="mt-3 mb-1 text-lg leading-6">Anu <br />Sharma</p>
            <p class="mb-3 text-[11px] text-gray-500">CO-CREATOR</p>
            <div class="flex gap-3 ml-3">
              <a target="_blank" href="https://www.linkedin.com/in/anu-sharma-6936a686/"
                ><img src={linkedin} alt="LinkedIn Icon" /></a
              >
              <a target="_blank" href="https://github.com/anulepau"
                ><img src={github} alt="GitHub Icon" /></a
              >
            </div>
          </div>
          <div class="contributor-card">
            <img src={andrew} alt="" class="hidden contributors:block h-28 rounded-full" />
            <p class="mt-3 mb-1 text-lg leading-6">Andrew <br />Widjaja</p>
            <p class="mb-3 text-[11px] text-gray-500">CO-CREATOR</p>
            <div class="flex gap-3 ml-3">
              <a target="_blank" href="https://www.linkedin.com/in/andrew-widjaja/"
                ><img src={linkedin} alt="LinkedIn Icon" /></a
              >
              <a target="_blank" href="https://github.com/andrew-widjaja"
                ><img src={github} alt="GitHub Icon" /></a
              >
            </div>
          </div>
        </div>
        <div class="flex justify-evenly w-1/2 m-2 gap-2 contributors:gap-0">
          <div class="contributor-card">
            <img src={aaron} alt="" class="hidden contributors:block h-28 rounded-full" />
            <p class="mt-3 mb-1 text-lg leading-6">Aaron <br />Willett</p>
            <p class="mb-3 text-[11px] text-gray-500">CO-CREATOR</p>
            <div class="flex gap-3 ml-3">
              <a target="_blank" href="https://www.linkedin.com/in/awillettnyc/"
                ><img src={linkedin} alt="LinkedIn Icon" /></a
              >
              <a target="_blank" href="https://github.com/awillettnyc"
                ><img src={github} alt="GitHub Icon" /></a
              >
            </div>
          </div>
          <div class="contributor-card">
            <img src={alexander} alt="" class="hidden contributors:block h-28 rounded-full" />
            <p class="mt-3 mb-1 text-lg leading-6">Alexander <br />Zambrano</p>
            <p class="mb-3 text-[11px] text-gray-500">CO-CREATOR</p>
            <div class="flex gap-3 ml-3">
              <a target="_blank" href="https://www.linkedin.com/in/alexander-z-8b7716b0/"
                ><img src={linkedin} alt="LinkedIn Icon" /></a
              >
              <a target="_blank" href="https://github.com/azambran21"
                ><img src={github} alt="GitHub Icon" /></a
              >
            </div>
          </div>
        </div>
      </div>
      <div class="flex flex-col contributors:flex-row my-4 items-center justify-evenly">
        <div class="flex justify-evenly w-1/2 m-2 gap-2 contributors:gap-0">
          <div class="contributor-card">
            <img src={justin} alt="" class="hidden contributors:block h-28 rounded-full" />
            <p class="mt-3 mb-1 text-lg leading-6">Justin <br />Wouters</p>
            <p class="mb-3 text-[11px] text-gray-500">CO-CREATOR</p>
            <div class="flex gap-3 ml-3">
              <a target="_blank" href="https://www.linkedin.com/in/justinwouters/"
                ><img src={linkedin} alt="LinkedIn Icon" /></a
              >
              <a target="_blank" href="https://github.com/justinwouters"
                ><img src={github} alt="GitHub Icon" /></a
              >
            </div>
          </div>
          <div class="contributor-card">
            <img src={damian} alt="" class="hidden contributors:block h-28 rounded-full" />
            <p class="mt-3 mb-1 text-lg leading-6">Damian <br />Lim</p>
            <p class="mb-3 text-[11px] text-gray-500">CO-CREATOR</p>
            <div class="flex gap-3 ml-3">
              <a target="_blank" href="https://www.linkedin.com/in/lim-damian/"
                ><img src={linkedin} alt="LinkedIn Icon" /></a
              >
              <a target="_blank" href="https://github.com/limd96"
                ><img src={github} alt="GitHub Icon" /></a
              >
            </div>
          </div>
        </div>
        <div class="flex justify-evenly w-1/2 m-2 gap-2 contributors:gap-0">
          <div class="contributor-card">
            <img src={walter} alt="" class="hidden contributors:block h-28 rounded-full" />
            <p class="mt-3 mb-1 text-lg leading-6">Walter <br />DeVault</p>
            <p class="mb-3 text-[11px] text-gray-500">CO-CREATOR</p>
            <div class="flex gap-3 ml-3">
              <a target="_blank" href="https://www.linkedin.com/in/walter-devault/"
                ><img src={linkedin} alt="LinkedIn Icon" /></a
              >
              <a target="_blank" href="https://github.com/TensionCoding"
                ><img src={github} alt="GitHub Icon" /></a
              >
            </div>
          </div>
          <div class="contributor-card">
            <img src={von} alt="" class="hidden contributors:block h-28 rounded-full" />
            <p class="mt-3 mb-1 text-lg leading-6">Von <br />Garcia</p>
            <p class="mb-3 text-[11px] text-gray-500">CO-CREATOR</p>
            <div class="flex gap-3 ml-3">
              <a target="_blank" href="https://www.linkedin.com/in/gerard-von-g-3964bb160/"
                ><img src={linkedin} alt="LinkedIn Icon" /></a
              >
              <a target="_blank" href="https://github.com/vongarcia97"
                ><img src={github} alt="GitHub Icon" /></a
              >
            </div>
          </div>
        </div>
        <div class="flex flex-col contributors:flex-row my-4 items-center justify-evenly">
          <div class="flex justify-evenly w-1/2 m-2 gap-2 contributors:gap-0">
            <div class="contributor-card">
              <img src={ali} alt="" class="hidden contributors:block h-28 rounded-full" />
              <p class="mt-3 mb-1 text-lg leading-6">Ali <br />Adams</p>
              <p class="mb-3 text-[11px] text-gray-500">CO-CREATOR</p>
              <div class="flex gap-3 ml-3">
                <a target="_blank" href="https://www.linkedin.com/in/alimadams/"
                  ><img src={linkedin} alt="LinkedIn Icon" /></a
                >
                <a target="_blank" href="https://github.com/AliA12336"
                  ><img src={github} alt="GitHub Icon" /></a
                >
              </div>
            </div>
          </div>
        </div>
      </div> -->

<!-- END ORIGINAL CARDS -->
<style>
  .css-blurry-gradient {
    overflow-x: hidden;
    position: absolute;
    top: 60%;
    left: 50%;
    transform: translate(-50%, 0%);
    will-change: transform;
    width: 100%;
    height: 100%;
    border-radius: 50% 22% 40% 80%;
    filter: blur(120px);
    background: radial-gradient(circle at 50% 50%, rgb(247, 85, 56), rgba(205, 203, 211, 0));
    opacity: 0.4;
    z-index: -1;
  }

  @media (max-width: 768px) {
    .css-blurry-gradient {
      opacity: 0.8;
      top: 50%;
    }
  }

  .wrapper {
    box-shadow: 3px 3px 15px 3px rgba(0, 0, 0, 0.1);
  }

  .btn {
    float: left;
    margin-left: 7px;
    width: 12px;
    height: 12px;
    border-radius: 100%;
  }
  .typing-demo {
    width: 20ch;
    animation: typing 4s steps(20), blink 0.5s step-end infinite alternate;
    animation-iteration-count: infinite;
    white-space: nowrap;
    overflow: hidden;
    border-right: 3px solid;
    font-family: monospace;
    font-size: 16px;
  }
  @media (max-width: 768px) {
    .typing-demo {
      animation: blink 0.5s step-end infinite alternate;
    }
  }

  @keyframes typing {
    0% {
      width: 0%;
    }
    25%,
    100% {
      width: 20ch;
    }
  }

  @keyframes blink {
    50% {
      border-color: transparent;
    }
  }
</style>
