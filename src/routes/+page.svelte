<script lang="ts">
  import { onMount } from "svelte";

  export let fontSize = 30;

  function muationObs() {
    const targetNode = document.body;

    const config = { childList: true };

    const callback = (mutationList: any) => {
      for (const mutation of mutationList) {
        if (mutation.type === "childList") {
          mutation.addedNodes.forEach((node: Node) => {
            if (node.nodeName === 'P') {
              handleParagraph(node);
            }
          })
        }
      }
    };

    const observer = new MutationObserver(callback);

    observer.observe(targetNode, config);
  }

  function handleParagraph(paragraph: Node) {
    const targetElement = document.getElementById("target");

    if (targetElement) {
      const pText = "・" + paragraph.textContent;
      paragraph.textContent = pText;
      targetElement.appendChild(paragraph); 
      console.log("A child node has been added.");
    }
  }

  function increaseFontSize() {
    fontSize++;
  }

  function decreaseFontSize() {
    fontSize--;
  }

  onMount(() => {
    muationObs();
  });
</script>

<h1 class="inline text-4xl">Font Size: {fontSize}</h1>
<button class="bg-sky-700 px-2 border-2 border-slate-200 rounded text-2xl" on:click={decreaseFontSize}>-</button>
<button class="bg-sky-700 px-2 border-2 border-slate-200 rounded text-2xl" on:click={increaseFontSize}>+</button>

<div style="font-size: {fontSize}px;" class="border-4 rounded-lg m-4 p-4" id="target">
  <p>・準備万端！！</p>
</div>