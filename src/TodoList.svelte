<script>
  import TodoItem from "./TodoItem.svelte";
  import {todos} from './store';
  import { onMount, onDestroy } from 'svelte';
  
  let text;
  let editActive = false;

  onMount(() => {
    let localArr = localStorage.getItem('todos')
    localArr && todos.set(JSON.parse(localArr))
  })

  const handleClick = () => {
    if (editActive) {
      alert("Подтвердите изменения!");
      return;
    } else if (text) {
      todos.update(arr => [...arr, { text, editCliсked: false }])
      text = "";
      localStorage.setItem('todos', JSON.stringify($todos))
    }
  };

  const handleRemove = event => {
    if (editActive) {
      alert("Подтвердите изменения!");
      return;
    }
    todos.update(arr => arr.filter((item, index) => index !== event.detail))
    localStorage.setItem('todos', JSON.stringify($todos))
  };

  const handleEdit = event => {
    editActive = true;
    text = event.detail.text;
    $todos[event.detail.index].text = "...";
    $todos[event.detail.index].editCliсked = true;
  };

  const handleEditApply = event => {
    if (text) {
      $todos[event.detail.index].text = text;
      text = "";
      $todos[event.detail.index].editCliсked = false;
      editActive = false;
      localStorage.setItem('todos', JSON.stringify($todos))
    } else {
      alert("Поле не может быть пустым!");
    }
  }; 
</script>

<div class="todoWrapper">
  <div class="inputWrapper">
    <label for="todo">Введите дело</label>
    <input type="text" id="todo" bind:value={text}>
  </div>
  <button on:click={handleClick}>Добавить дело</button>
  <ul>
    {#each $todos as item, i}
      <TodoItem 
        text={item.text} 
        index={i}
        clicked={item.editCliсked} 
        on:remove={handleRemove} 
        on:edit={handleEdit} 
        on:editApply={handleEditApply}/>
    {/each}
  </ul>
</div>

<style>
  .todoWrapper {
    width: 300px;
    margin: auto;
  }

  .inputWrapper {
    display: flex;
    flex-direction: column;
  }

  input {
    margin-top: 10px;
    padding: 5px 15px;
    border-radius: 15px;
    border: 1px solid;
  }

  button {
    margin-top: 10px;
    width: inherit;
    cursor: pointer;
  }

  ul {
    list-style-type: none;
    margin: 0;
    padding: 0;
    margin-top: 10px;
  }
</style>