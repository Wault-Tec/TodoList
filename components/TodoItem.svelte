<script>
  import { createEventDispatcher } from "svelte";

  export let text;
  export let index;
  export let clicked;
  const dispatch = createEventDispatcher();
</script>

<li>
  <div class="itemNumber">
    {index + 1}
  </div>
  <span>
    {text}
  </span>
  <div class="btnWrapper">
    <a on:click={() => dispatch('remove', index)}>Удалить</a>
    {#if clicked}
      <a on:click={() => dispatch('editApply', {text, index,clicked})}>ОК</a>
    {:else}
      <a on:click={() => dispatch('edit', {text, index,clicked})}>Изменить</a>
    {/if}
  </div>
</li>

<style>
  li {
    display: flex;
    justify-content: space-between;
    position: relative;
    margin-top: 10px;
    padding: 10px;
    background-color: aquamarine;
    border-radius: 10px;
    outline: 1px solid rgba(0, 0, 0, 0.3);
  }

  a {
    cursor: pointer;
    border: 1px solid rgba(0, 0, 0, 0.3);
    padding: 5px;
    border-radius: 10px;
    margin-top: 5px;
    width: inherit;
    background-color: rgb(58, 209, 159);
  }

  span {
    word-wrap: break-word;
    max-width: 190px;
    padding-top: 15px;
    text-align: left;
  }

  .btnWrapper {
    width: 80px;
    display: flex;
    flex-wrap: wrap;
  }

  .itemNumber {
    position: absolute;
    width: 20px;
    height: 20px;
    box-sizing: border-box;
    border: 1px solid rgba(0, 0, 0, 0.3);
    border-radius: 50%;
    left: 2px;
    top: 2px;
    font-size: 12px;
    line-height: 20px;
  }

</style>