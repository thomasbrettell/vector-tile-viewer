<script lang="ts">
  import { type VectorTile } from "@mapbox/vector-tile";
  import Tile from "./Tile.svelte";
  import * as d3 from "d3";

  let { tile }: { tile: VectorTile } = $props();

  let width = $state<number>();
  let height = $state<number>();
  let size = $derived(width && height ? Math.min(width, height) : null);

  const color = d3
    .scaleOrdinal<string>()
    .domain(Object.keys(tile.layers))
    .range([...d3.schemeCategory10, ...d3.schemeTableau10]);
</script>

<div class="wrapper">
  <!-- <div class="layers-info">
    {#each Object.keys(tile.layers) as layerName}
      <div class="layer-info" style:background-color={color(layerName)}>
        {layerName}
      </div>
    {/each}
  </div> -->
  <div class="view" bind:clientWidth={width} bind:clientHeight={height}>
    {#if size}
      <div>
        <Tile {tile} {size} {color} />
      </div>
    {/if}
  </div>
</div>

<style lang="scss">
  // .layers-info {
  //   display: flex;
  //   flex-direction: column;
  //   overflow-y: auto;
  //   gap: 2.5px;
  // }

  .layer-info {
    padding: 0.2em;
    width: 100%;
  }

  .wrapper {
    display: flex;
    width: 100%;
    height: 100%;
    padding: 1em;
    overflow: hidden;
  }

  .view {
    flex: 1;
    flex-basis: 100%;
    overflow: hidden;
    display: flex;
    justify-content: center;
  }
</style>
