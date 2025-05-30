<script lang="ts">
  import { onMount } from 'svelte';

  let light;
  let messageElement;
  const messages = [
    "An independent, community-owned Google Earth, free from centralized servers and overpowered entities, owned solely by you and the public!",
    "IT'S FREE! More users mean more app storage and computational power. No back-end needed! Syncs via public tracker networks. Now using BitTorrent, with Nostr as a fallback coming soon...",
    "Decentralization places the globe within your grasp, ensuring your voice resonates daily, not merely at the ballot box every few years. It’s about making your voice count every day, not just delegating it during elections.",
    "Pick what suits your flow: Unreal Engine 5, Unity 3D, HTML, CSS, JS, APIs (JAMstack), WASM or WebContainers.",
    "Reach out to me and our community anytime on GitHub, Gitter.im, or during our upcoming weekly Zoom brainstorming sessions on YouTube.",
    "They always said it’s impossible and then one came who didn’t know that and just did<br> - Multiple Authors",
    "A function of civil society is to organize ourselves to collectively stand up to those who add to the suffering of others.<br> - Ira Chaleff",
    "Get paid for the subject you love!<br> - Cloud Atlas",
    "Create and participate in missions addressing <strong>local and global</strong> issues. <strong>Brainstorm (Zoom.us)</strong> solutions, realize them with <strong>ActionEvents (Telegram)</strong>, enhance public spaces via <strong>Petition (change.org)</strong>, and turn your ideas into reality with <strong>Crowdfunding (gofundme.com).</strong>",
    "Make the world work for 100% of humanity, in the shortest possible time, through spontaneous cooperation, without ecological offense or the disadvantage of anyone.<br> - B. Fuller",
    "The Settings Icon (BOTTOM RIGHT) enables you to propose, create, and implement new applications, enhancements, and bug fixes directly - in Browser - all without the need for installing a coding environment. Contributions can be made effortlessly via a simple pull request.",
    "You never change things by fighting the existing reality. To change something, build a new model that makes the existing model obsolete.<br>- Buckminster Fuller",
  ];
  let fadeDuration = 5; // Duration in seconds

  function moveLight() {
    const vw = Math.max(document.documentElement.clientWidth || 0, window.innerWidth || 0);
    const vh = Math.max(document.documentElement.clientHeight || 0, window.innerHeight || 0);

    const edge = Math.floor(Math.random() * 4);
    let startX, startY, endX, endY;

    switch (edge) {
      case 0: // top
        startX = Math.random() * vw;
        startY = 0;
        endX = Math.random() * vw;
        endY = vh;
        break;
      case 1: // right
        startX = vw;
        startY = Math.random() * vh;
        endX = 0;
        endY = Math.random() * vh;
        break;
      case 2: // bottom
        startX = Math.random() * vw;
        startY = vh;
        endX = Math.random() * vw;
        endY = 0;
        break;
      case 3: // left
        startX = 0;
        startY = Math.random() * vh;
        endX = vw;
        endY = Math.random() * vh;
        break;
    }

    if (light) {
      light.style.left = `${startX}px`;
      light.style.top = `${startY}px`;
    }

    const distance = Math.sqrt(Math.pow(endX - startX, 2) + Math.pow(endY - startY, 2));
    light.style.transition = `all ${distance / 100}s linear`;

    setTimeout(() => {
      if (light) {
        light.style.left = `${endX}px`;
        light.style.top = `${endY}px`;
      }
    }, 100);
  }

  function showMessage() {
    const vw = Math.max(document.documentElement.clientWidth || 0, window.innerWidth || 0);
    const vh = Math.max(document.documentElement.clientHeight || 0, window.innerHeight || 0);

    const messageWidth = 300; // Adjust this value based on your message width
    const messageHeight = 500; // Adjust this value based on your message height

   // Ensure the message doesn't overlap with other components by adding a X px padding
   // dont now how it works but it does its job
  const x = 50 + Math.random() * (vw - messageWidth - 100); // Subtract 100px for left and right padding
  const y = 50 + Math.random() * (vh - messageHeight + 100); // Add 100px for top and bottom padding
  

    if (messageElement) {
      messageElement.style.left = `${x}px`;
      messageElement.style.top = `${y}px`;

      // Choose a random message index
      const randomIndex = Math.floor(Math.random() * messages.length);
      messageElement.innerHTML = messages[randomIndex]; // Set the innerHTML directly

      messageElement.style.transition = `opacity ${fadeDuration}s ease-in-out`;
      messageElement.style.opacity = '1';

      setTimeout(() => {
        messageElement.style.opacity = '0';
        setTimeout(showMessage, 5000); // wait for a short time before showing the next message
      }, 10000); // show each message for x seconds
    }
  }

  onMount(() => {
    setInterval(moveLight, 6000);
    showMessage();
  });
</script>

<div class="container">
  <div id="hex-grid">
    <div bind:this={light} class="light"></div>
    <div bind:this={messageElement} class="message"></div>
    <div class="grid"></div>
  </div>
</div>

<style>
  .container {
    background: #2b2b2b;
    display: flex;
    justify-content: center;
    align-items: center;
    min-height: 100vh;
    position: relative;
    z-index: 0;
  }

  #hex-grid {
    height: 100vh;
    background: #000;
    z-index: 1;
  }

  #hex-grid .grid {
    position: absolute;
    top: 0;
    left: 0;
    background: url(/public/grid.svg) repeat;
    width: 100%;
    height: 100%;
    z-index: 3;
    background-size: 500px;
  }

  #hex-grid .light {
    position: absolute;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
    width: 15em;
    height: 15em;
    filter: blur(15px);
    background: linear-gradient(90deg, #335bf4 0%, #2ae9c9 100%);
    z-index: 2;
  }

  #hex-grid .message {
    position: absolute;
    width: 300px; /* Adjust based on message length */
    height: 500px; /* Adjust based on message height */
    font-size: 1.5em;
    color: #fff;
    text-align: center;
    line-height: 50px;
    opacity: 0;
    transition: opacity 5s ease-in-out; /* Adjust the duration here */
    z-index: 4;
    font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', 'Roboto', 'Oxygen',
    'Ubuntu', 'Cantarell', 'Fira Sans', 'Droid Sans', 'Helvetica Neue', sans-serif;
    font-size: larger;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    text-align: center;
    color: grey;
    padding: 0%;
    margin: 0%;
    text-decoration: none;
    box-sizing: border-box;
  }
</style>
