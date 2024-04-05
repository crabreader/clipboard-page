<script lang="ts">
  import { onMount } from "svelte";
  import Button from "../components/Button.svelte";

  const CHARS_TO_IGNORE = "「」『』[]()〈〉≪≫。、.,'：！?！？…――─ｰ～→♪" + '"' + "　" + "  "
  
  export let fontSize = 30;
  export let autoScroll = true;
  export let lines: string[] = [];
  export let chars = 0;


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

              if (node.parentNode) {
                node.parentNode.removeChild(node);
              }
            }
          });
        }
      }
    };

    const observer = new MutationObserver(callback);

    observer.observe(targetNode, config);
  }

  function handleParagraph(paragraph: Node) {

    const pText = paragraph.textContent;

    if (pText) {
      lines = [...lines, pText];
      chars += countChars(pText);

      localStorage.setItem("lines", JSON.stringify(lines));
    }
  }

  function countChars(line: string) {
    let filtered = "";

    for (let i = 0; i < line.length; i++) {
      let ignore = false;

      for (let j = 0; j < CHARS_TO_IGNORE.length; j++) {
        if (line[i] === CHARS_TO_IGNORE[j]) {
          ignore = true;
        }      
      }

      if (!ignore) {
        filtered += line[i];
      }
    }

    return filtered.length;
  }

  function toggleAutoscroll() {
    autoScroll = !autoScroll;
  }

  function increaseFontSize() {
    fontSize += 2;
  }

  function decreaseFontSize() {
    fontSize -=2;
  }

  function deleteLastLine() {
    chars -= countChars(lines[lines.length - 1]);
    lines = [...lines.slice(0, lines.length - 1)];
    localStorage.setItem("lines", JSON.stringify(lines));
  }

  function deleteAllLines() {
    lines = [];
    localStorage.removeItem("lines");
  }

  onMount(() => {
    muationObs();

    const localLines = localStorage.getItem("lines");
    if (localLines) {
      lines = JSON.parse(localLines);

      lines.forEach(line => {
        chars += countChars(line);
      });
    }
  });
</script>

<div class="p-4 sticky top-0 backdrop-blur-xl">
  <h1 class="text-2xl inline">Font Size: {fontSize}</h1>

  <Button content="-" onClick={decreaseFontSize} />
  <Button content="+" onClick={increaseFontSize} />

  <Button content="Toggle autoscroll" onClick={toggleAutoscroll} />

  <Button content="Delete all lines" onClick={deleteAllLines} />

  <Button content="Delete last line" onClick={deleteLastLine} />

  <h1 class="text-2xl inline">{lines.length} / {chars}</h1>
</div>

<div style="font-size: {fontSize}px;" class="border-4 rounded-lg m-4 p-4" id="target">
  <p>(´• ω •`)ﾉ</p>
  {#each lines as line}
    <p><span class="text-slate-400">・</span>{line}</p>
  {/each}
</div>