<div class="stack-container">
  <h4>Stack</h4>
  <div class="stack">
    {#each uiStack as el (el.id)}
		<div
      class="element"
      transition:fly="{{ y:-50 }}"
    >{el.id + ': ' + el.value}</div>
  	{/each}  
  </div>
</div>

<script>
  import { fly } from 'svelte/transition'

	// logic goes here
  console.log('component/stack.svelte')
  export let stack = []
  $: uiStack = stack.slice().reverse()
</script>

<style>
.stack-container {
  padding-right: 8px;
}

.stack {
  height: 300px;
  width: 300px;
  display: table-cell;
  vertical-align: bottom;
  padding: 4px;
  color: white;
  background-color: #EEE;
}

.stack > .element {
  border-radius: 4px;
  margin: 4px;
  padding: 4px;
  background-color: dodgerblue;
  color: white;
}
</style>