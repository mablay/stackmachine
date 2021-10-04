<div>
  <h4>Heap</h4>
  <pre>{uiHeap}</pre>
</div>

<script>
  export let heap = [3, 99, 7, 2]
  // $: uiHeap = heap.map((n, i) => `${i}: ` + n.toString(16).padStart(2, '0').toUpperCase())
  $: uiHeap = heap.map(n => n.toString(16).padStart(2, '0').toUpperCase()).join('')
</script>

<style>
</style>
