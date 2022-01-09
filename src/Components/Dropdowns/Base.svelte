<script>
  import { afterUpdate, onDestroy, onMount } from "svelte";
  import { createEventDispatcher } from 'svelte';

  const dispatch = createEventDispatcher();

  export let anchor;
  export let disposable;
  export let disabled = false;
  export let dropdownClass = '';
  export let share;
  export let containerClass = 'relative';

  const ANCHORS = ["left", "right"];
  const DISPOSABLES = ["passive", "active", "aggressive"];
  
  let show = false
  let disposables = DISPOSABLES
  let anchors = ANCHORS
  let heightLabel = 0
  let isTouchBottom = false
  let isTouchTop = false
  let position = ""

  function toggle() {
    if (disabled) return;
    show = !show;
  }

  function showMenu() {
    setTimeout(() => {
      show = true;

      // $nextTick(() => {
      //   onDropdownOveflow();
      // });
    }, 200);
  }

  function hideMenu({ uid, callback }) {
    //is self
    if (uid === _uid) {
      show = false;
      dispatch("menu-hide");
    }

    if (callback) callback();
  }

  function updateHeightLabel() {
    heightLabel = this.querySelector('#label').offsetHeight;
  }

  function onDropdownClick() {
    if (disposable === disposables[2]) {
      hideMenu({ uid: _uid });
    }
  }

  function clickOutside() {
    if (!show) return;

    const specifiedElement = this.querySelector('#dropdownContainer');
    const isClickInside = specifiedElement.contains(event.target);
    if (isClickInside) {
    } else {
      hideMenu({ uid: _uid });
    }

    // $nextTick(() => {
    //   onDropdownOveflow();
    // });
  }

  function onDropdownOveflow() {
    let limitBottomCoor;
    let limitTopCoor;
    let containerCoor;

    const {
      height: dropmenuHeight,
      top: dropdownTop,
    } = this.querySelector('#dropmenu').getBoundingClientRect();

    const {
      top: containerTop,
      bottom: containerBottom,
    } = this.querySelector('#dropdownContainer').getBoundingClientRect();

    containerCoor = (containerTop + containerBottom) / 2;

    const containerNearby = hasClass(
      this.querySelector('#dropmenu'),
      "container-wrap"
    );
    if (containerNearby) {
      const { bottom, top } = containerNearby.getBoundingClientRect();
      limitBottomCoor = bottom;
      limitTopCoor = top;
    } else {
      limitBottomCoor = window.innerHeight;
      limitTopCoor = 0;
    }

    if (
      containerCoor + dropmenuHeight > limitBottomCoor &&
      containerCoor - dropmenuHeight < limitTopCoor
    ) {
      isTouchBottom = true;
      isTouchTop = true;
    } else if (containerCoor + dropmenuHeight > limitBottomCoor) {
      isTouchBottom = true;
    } else if (containerCoor - dropmenuHeight < limitTopCoor) {
      isTouchTop = true;
    } else {
      isTouchBottom = false;
      isTouchTop = false;
    }
  }
  
  function hasClass(element, className) {
    do {
      if (element.className?.split(" ").includes(className)) {
        return element;
      }
      element = element.parentNode;
    } while (element);
    return false;
  }

  onMount(() => {
    updateHeightLabel();

    window.addEventListener("click", clickOutside, false);
  });

  afterUpdate(() => {
    updateHeightLabel();
  });

  onDestroy(() => {
    window.removeEventListener("click", clickOutside, false);
  })
</script>

<div :id="_uid" id="dropdownContainer" class={ containerClass }>
  <div id="label" class="flex h-full items-center" on:click={ toggle }>
    <div class="flex h-full w-full">
      {#if show }
        <slot name="label" />
      {/if}
    </div>
  </div>
  <div
    v-show="show"
    id="dropmenu"
    class="dropdown-container {dropdownClass}"
    class:right-0={ anchor === anchors[1] }
    class:left-0={ anchor === anchors[0] }
    class:min-width-0={ share }
    style={ isTouchBottom ? `bottom: ${heightLabel}px` : null }
  >
    <div on:click={ onDropdownClick }>
      <slot name="dropdown" />
    </div>
  </div>
</div>

<style lang="postcss" scoped>
.dropdown-container {
  @apply absolute my-2 rounded-md shadow-lg z-10 bg-white border-gray-50;
  min-width: 12rem;
}
.min-width-0 {
  min-width: 0;
}
</style>
