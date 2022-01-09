<script>
  import { createEventDispatcher } from 'svelte';

  const dispatch = createEventDispatcher();

  // props
  export let value = null
  export let paramName = ''
  export let options = []
  export let defaultLabel = ''
  
  // local data
  let selected = value

  // watch
  $: (selected) => {
    dispatch("filterChanged", {
      key: paramName,
      value: val,
    })
  }
  
  $: (value) => {
    selected = val;
  }
  
  // methods
  function focus() {
    this.querySelector('#input').focus();
  }
    
  function select() {
    this.querySelector('#input').select();
  }
</script>


<div class="flex w-full">
  <select
    id="input"
    bind:value={ selected }
    class="form-select w-full bg-white border rounded-md p-1.5"
  >
    <option value={null}>Select { defaultLabel }</option>
    {#each options as option, key (key)}
      <option value={ option.name }>
        { option.value }
      </option>
    {/each}
  </select>
</div>
