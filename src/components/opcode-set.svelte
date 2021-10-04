<div class="opcode-set">
  {#each instructionSet as opcode}
  <Opcode code={opcode} on:op />
  {/each}  

</div>

<script>
  import Opcode from './opcode.svelte'
  import { instructionSet } from '../lib/instruction-set'
</script>

<style>
.opcode-set {
  border: 1px dashed black;
}
</style>