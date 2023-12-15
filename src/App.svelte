<script>
  import { Input, Label, Button, Heading, Checkbox } from 'flowbite-svelte'; 
  import Todo from './Todo.svelte'

  let todos = [
    {
      name: 'First todo',
      isDone: false
    }
  ]

  let newTodo = ''

  let isShowAllTodos = false

  function addTodo(e) {

    if (e.key && e.key !== 'Enter') return 

    todos = [...todos, {
      name: newTodo,
      isDone: false
    }]

    newTodo = ''
  }

  function removeTodo(i) {
    todos.splice(i, 1)
    todos = todos
  }

  function onChange(e, todo) {
    todos = todos.map(t => {
      if (t === todo) {
        return {...todo, ...e.detail}
      }
      return t
    })
  }

  $: filteredTodos = todos.filter(t => {
    if (isShowAllTodos) return true
    return !t.isDone
  })

</script>

<Heading tag="h1" class="mb-4" customSize="text-4xl font-extrabold md:text-5xl lg:text-6xl text-center">
  Todo App
</Heading>

<Checkbox class="mb-5" bind:checked={isShowAllTodos}>Show all todos</Checkbox>

<ul class="mb-5">
  {#each filteredTodos as todo, i}
    <Todo 
      {todo} 
      on:change={(e) => onChange(e, todo)}
      on:delete={() => removeTodo(i)}
    />
  {/each}
</ul>

<div>
  <Label for="new_todo" class="mb-2">New todo</Label>
  <div class="new-todo">
    <Input class="mr-2" type="text" id="new_todo" placeholder="New todo" bind:value={newTodo} on:keydown={addTodo} />
    <Button on:click={addTodo}>Add</Button>
  </div>
</div>

<style>

  .new-todo {
    display: flex;
  }

  ul {
    list-style: none; 
    min-height: 250px;
    max-height: 250px;
    overflow-y: auto;
    padding: 0 15px;
  }
</style>