<script lang="ts">
  import { createEventDispatcher } from 'svelte'
  import { getItemIndex, List, ListItem } from "../utils/lists"

  let dispatch = createEventDispatcher()

  export let list: List
  export let item: ListItem
  export let donabled = false
  let testIndex = -1

  function onMenu() {
    let index = getItemIndex(list, item)
    testIndex = index
    dispatch("setIndex", index)
    dispatch("menu", { index })
  }

  const onOpen = () => {
    testIndex = getItemIndex(list, item)
    dispatch("setIndex", -1)
    dispatch("open", item)
  }

  function onDone() {
    let index = getItemIndex(list, item)
    testIndex = index
    dispatch("setIndex", index)
    dispatch("done", { index })
  }

  export let completed = false
  export let isMoving = false
  export let selected = false

  function classLabel() {
    return ["list-item", completed ? "item-completed" : ""].join(" ")
  }

  function classRow() {
    return isMoving && selected ? "row-selected" : ""
  }
</script>

<dockLayout
  stretchLastChild="{true}"
  colSpan="3"
  class="{classRow()}"
  >
  {#if donabled}
    <button
      class="button-done"
      dock="left"
      width="30"
      text="{"👍"}"
      on:tap="{onDone}"
    />
  {/if}

  {#if item.subdivisions}
    <button
      class="button-abrir"
      dock="right"
      width="50"
      text="{"📁 "+item.subdivisions.length}"
      on:tap="{onOpen}"
    />
  {/if}
  <!-- svelte-ignore a11y-label-has-associated-control -->
  <label
    class="{classLabel()}"
    text="{item.name}"
    textWrap="{true}"
    on:tap="{onMenu}"
  />
</dockLayout>

<style>
  .list-item {
    font-family: 'KoHo', "KoHo-SemiBold", sans-serif;
    font-size: 20;

    color: #262626;
    color: #3c495e;

    padding: 4 24;
  }

  .button-abrir {
    font-size: 16;

    border-radius: 5;
    height: 32;

    padding: 0;
    margin-right: 12;
  }

  .button-done {
    font-size: 16;

    border-radius: 25%;
    height: 32;

    padding: 0;
    margin-right: 0;
    margin-left: 12;
  }

  .item-completed {
	  color: #939393;
	  text-decoration: line-through;
  }

  .row-selected {
    color: #3c495e;
    background: #eee;
  }
</style>
