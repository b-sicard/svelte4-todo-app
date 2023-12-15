<script>
  import { createEventDispatcher } from 'svelte'
  import { fade } from 'svelte/transition'
  import { Input, Checkbox, Button } from 'flowbite-svelte'; 

  const dispatch = createEventDispatcher()

  export let todo = {};

  function onCheck() {
    dispatch('change', {
      isDone: !todo.isDone
    })
  }

  function onInput() {
    dispatch('change', {
      name: todo.name
    })
  }

  function onDelete() {
    dispatch('delete')
  }

</script>

<li transition:fade class="mb-6">    
  <Checkbox
    class="space-x-1 rtl:space-x-reverse mr-2"
    checked={todo.isDone}
    on:change={onCheck}
    id="is_done"
  >
    <Input
      type="text"
      bind:value={todo.name}
      on:input={onInput}
      id="name"
    />
  </Checkbox>
  <Button on:click={onDelete}>X</Button>
</li>

<style>
  li {
    display: flex;
    align-items: center;
    justify-content: center;
  }
</style>