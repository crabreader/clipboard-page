<script>
  import "../app.css";
</script>

<div class="container m-4">
  <slot />
</div>

<style lang="postcss">
  :global(html) {
    background-color: theme(colors.slate.800);
    color: theme(colors.stone.200);
  }
</style>