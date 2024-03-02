<script>
// @ts-nocheck

  export let showForm;
</script>

<nav class="nav">
  <div class="nav-center">
    <h1 class="nav-title">budget calculator</h1>
    <a href="../routes/about/+page.svelte">
    <button type="button" class="nav-btn" on:click={showForm}>
     
      add item
    </button>
  </a>
  </div>
</nav>
