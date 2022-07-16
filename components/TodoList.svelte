<script>
  import TodoItem from "./TodoItem.svelte";

  let text;
  let todoItems = [];
  let editActive = false;

  const handleClick = () => {
    if (editActive) {
      alert("Подтвердите изменения!");
      return;
    } else if (text) {
      todoItems = [...todoItems, { text, editCliсked: false }];
      text = "";
    }
  };

  const handleRemove = event => {
    if (editActive) {
      alert("Подтвердите изменения!");
      return;
    }
    todoItems.splice(event.detail, 1);
    todoItems = todoItems;
  };

  const handleEdit = event => {
    editActive = true;
    text = event.detail.text;
    todoItems[event.detail.index].text = "...";
    todoItems[event.detail.index].editCliсked = true;
  };

  const handleEditApply = event => {
    if (text) {
      todoItems[event.detail.index].text = text;
      text = "";
      todoItems[event.detail.index].editCliсked = false;
      editActive = false;
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
    {#each todoItems as item, i}
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