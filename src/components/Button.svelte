<script lang="ts">
    export let content: string;
    export let onClick: () => void;
</script>

<button class="bg-slate-500 hover:bg-slate-700 active:ring-slate-400 text-white font-bold py-2 px-4 m-1 rounded ring" on:click={onClick}>{content}</button>
