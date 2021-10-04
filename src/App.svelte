<div>
  <Program bind:prog={prog} />
  <button on:click={parse}>Parse</button>
  <button on:click={next} disabled={isAnimating}>Next</button>
  <div class="row">
    <div class="column">
      <Stack stack="{stack}"></Stack>
    </div>
    <div class="column">
      <Heap heap="{heap}"></Heap>
    </div>
  </div>
  <OpcodeSet on:op={doOp} />
</div>
<script>
import Stack from './components/stack.svelte'
import Heap from './components/heap.svelte'
import OpcodeSet from './components/opcode-set.svelte'
import Program from './components/prog.svelte'
import { instructionSet } from './lib/instruction-set'
export let stack = []
export let heap = [10, 20, 3, 45]
export let prog = '6 2 3 5 + - ='

let isAnimating = false
const delay = ms => new Promise(resolve => setTimeout(resolve, ms))

let seq = []

const opMap = {
  push () {
    console.log('op: PUSH')
  },
  pop () {
    console.log('op: POP')
  },
  async equals () {
    const a = Number(stack.pop().value)
    const b = Number(stack.pop().value)
    stack = stack
    await delay(500)
    await pushData(a === b)
    // stack = stack
  },
  async add () {
    const a = Number(stack.pop().value)
    const b = Number(stack.pop().value)
    stack = stack
    await delay(500)
    await pushData(a + b)
    // stack = stack
  },
  async subtract () {
    const a = Number(stack.pop().value)
    const b = Number(stack.pop().value)
    stack = stack
    await delay(500)
    await pushData(a - b)
    // stack = stack
  }
}

parse()

// iterate stack if there is sth. to do
async function next () {
  isAnimating = true
  const tosreg = peekTosreg()
  if (tosreg?.type === 'opcode') {
    // perform operation if there is one on top of the stack
    stack.pop()
    const op = opMap[tosreg.value]
    await op()

    // check for program exit state
    if (seq.length === 0 && stack.length === 1) {
      if (stack[0]) {
        console.log('SUCCESS!')
      } else {
        console.log('FAILED!')
      }
    }
  } else {
    // continue in the program otherwise
    if (seq.length > 0) {
      await push(seq.shift())
    }
  }
  isAnimating = false
}

// peeks the Top Of Stack REGister
function peekTosreg () {
  if (stack.length === 0) return
  return stack[stack.length - 1]
}

// user clicked on opcode, place opcode in prog sequence
function doOp ({ detail: opcode }) {
  const name = opcode.name
  const op = opMap[name]
  if (!op) {
    return console.log('un-mapped operation:', name)
  }
  prog = prog.trim().concat(' ', opcode.operation)
}

function parse () {
  stack = []
  seq = []
  const instructions = prog.split(' ')
  for (const s of instructions) {
    const opcode = instructionSet.find(x => x.operation === s)
    // console.log('PARSE:', {opcode})
    if (opcode) {
      // op_code
      // console.log(`PROG: OP_${opcode.name}`)
      seq.push({ type: 'opcode', value: opcode.name })
      // push(opcode.name)
    } else {
      // data
      // console.log(`PROG: > ${s}`)
      seq.push({ type: 'data', value: s })
      // push(s)
    }
  }
  console.log('program:', seq)
}

let elId = 0
async function push ({ type, value }) {
  const element = {
    id: elId++,
    type,
    value
  }
  stack = [...stack, element]
  await delay(500)
}

function pushData (value) {
  push({ type: 'data', value })
}

function pop () {
  stack.splice(stack.length - 1, 1)
  stack = stack
}
</script>
<style>
.column {
  float: left;
  width: 50%;
}

/* Clear floats after the columns */
.row:after {
  content: "";
  display: table;
  clear: both;
}
</style>
