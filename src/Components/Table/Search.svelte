<script>
  import { createEventDispatcher } from 'svelte';
  import debounce from 'lodash/debounce'

  const dispatch = createEventDispatcher();

  export let value = ''
  
  const handleInput = debounce(e => {
    value = e.target.value;
    dispatch("input", value);
	}, 300)
  </script>


<div class="flex w-full">
  <input
    ref="input"
    bind:value={ value }
    class="block form-input relative w-full focus:shadow-outline py-2 px-2 rounded text-base"
    autocomplete="off"
    type="text"
    name="search"
    placeholder="Search…"
    on:input={ handleInput }
  />
</div>
