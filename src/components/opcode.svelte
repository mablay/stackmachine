<div class="opcode" on:click={() => dispatch('op', code)}>
  <div class="code">{code.operation}</div>
  <div class="name">{name}</div>
</div>

<script>
  import { createEventDispatcher } from 'svelte'
  const dispatch = createEventDispatcher()

  export let code = 'push'
  $: name = code.name.toUpperCase()
</script>

<style>
.opcode {
  user-select: none;
  display: inline-block;
  background-color: #EEE;
  border-radius: 5px;
  color: deeppink;
  width: 100px;
  height: 100px;
  line-height: 32px;
  margin: 4px;
  text-align: center;
}

.opcode > .code {
  font-size: 32px;
  line-height: 64px;
}
</style>