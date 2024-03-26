<script lang="ts">
  import { onMount } from "svelte";
  import Button from "../components/Button.svelte";

  export let fontSize = 30;
  export let autoScroll = true;

  function scrollToBottom() {
      window.scrollTo(0, document.body.scrollHeight);
  }

  function muationObs() {
    const targetNode = document.body;

    const config = { childList: true };

    const callback = (mutationList: any) => {
      for (const mutation of mutationList) {
        if (mutation.type === "childList") {
          mutation.addedNodes.forEach((node: Node) => {
            if (node.nodeName === 'P') {
              handleParagraph(node);
              if (autoScroll) {
                scrollToBottom();
              }
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

<div class="p-4 sticky top-0 backdrop-blur-md hover:backdrop-blur-xl">
  <h1 class="text-2xl inline">Font Size: {fontSize}</h1>

  <Button content="-" onClick={decreaseFontSize} />
  <Button content="+" onClick={increaseFontSize} />

  <h1 class="text-2xl inline">Autoscroll: </h1>
  <input type="checkbox" id="myCheckbox" class="form-checkbox h-5 w-5 text-blue-600 border-gray-300 rounded focus:ring-blue-500" bind:checked={autoScroll}>
</div>

<div style="font-size: {fontSize}px;" class="border-4 rounded-lg m-4 p-4" id="target">
  <p>(´• ω •`)ﾉ</p>
</div>