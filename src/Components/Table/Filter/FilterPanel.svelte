<script>
  // components
  import FieldLabel from "../../Fields/Label.svelte";
  import DropdownBase from "../../Dropdowns/Base.svelte";
  import DropdownFilter from "./DropdownFilter.svelte";
  
  // props
  export let filters = []
  export let value = {}

  // methods
  function setFilter(filter) {
    dispatch("input", value);
  }

  function clear() {
    
  }

  function get(object, key) {
    return object[key] ?? null;
  }

  // computed
  function filtered() {
    for (let i in filters) {
      if (get(value, filters[i].paramName) != null) {
        return true;
      }
    }
    return false;
  }

  function filtersAndValues() {
    return filters.map((filter) => {
      filter.value = get(value, filter.paramName);
      return filter;
    });
  }
</script>

{#if filters.length > 0}
  <DropdownBase class="text-base" anchor={ 'left' }>
    <button
      type="button"
      class="
        bg-white
        border
        rounded-md
        focus:outline-none
        focus:border-primary-300
        focus:shadow-outline-primary
        flex
        py-1
        px-2
        flex
        { !filtered ? `bg-white` : `` }
        { filtered ? `bg-primary-100 border-primary-200` : `` }
      "
    >
      <span class="my-auto">Filter</span>
    </button>

    <div class="p-2 filters">
      <div class="flex">
        <field-label class="text-gray-500"> Filters </field-label>
        {#if filtered }
          <button
            class="text-xs text-gray-500 ml-auto"
            on:click={ clear }
          >
            (clear)
          </button>
        {/if }
      </div>
      {#each filtersAndValues as filter, key (key) }
        <DropdownFilter
          class="mt-2"
          value={ filter.value }
          defaultLabel={ filter.label }
          options={ filter.options }
          paramName={ "filter.paramName" }
          on:filterChanged={ setFilter }
        />
      {/each}
    </div>
  </DropdownBase>
{/if}

<style>
.filters {
  min-width: 18rem;
}
</style>
