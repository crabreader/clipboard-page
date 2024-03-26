<script lang="ts">
  import { onMount } from "svelte";

  function muationObs() {
    const targetNode = document.body;

    const config = { childList: true };

    const callback = (mutationList: any, observer: any) => {
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
      targetElement.appendChild(paragraph); 
      console.log("A child node has been added.");
    }
  }

  onMount(() => {
    muationObs();
  });
</script>

<h1 class="text-3xl font-bold underline">Welcome to SvelteKit</h1>
<p>Visit <a href="https://kit.svelte.dev">kit.svelte.dev</a> to read the documentation</p>

<div class="border-4 rounded-lg m-4 p-4" id="target">
  <p>Example</p>
</div>