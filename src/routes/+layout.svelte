<script>
  import "../app.css";
</script>

<slot />

<style lang="postcss">
  :global(html) {
    background-color: theme(colors.slate.800);
    color: theme(colors.stone.200);
  }
</style>