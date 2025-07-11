<script>


  const { label } = $props();
</script>

<span class="label">{label}</span>

<style>
  .label {
    padding: 4px 20px;
    /* background-color: var(--slate-gray-200); */
    border: 1px solid black;
    display: flex;
    width: fit-content;
    justify-content: center;
    align-items: center;
    font-size: 14px;
    font-weight: bold;
    letter-spacing: 1.2px;

  }
</style>