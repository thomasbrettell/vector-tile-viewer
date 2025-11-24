<script lang="ts">
  import { type VectorTile } from "@mapbox/vector-tile";
  import Tile from "./Tile.svelte";

  let { tile }: { tile: VectorTile | null } = $props();

  let width = $state<number>();
  let height = $state<number>();
  let size = $derived(width && height ? Math.min(width, height) : null);
</script>

<div class="wrapper">
  <div class="view" bind:clientWidth={width} bind:clientHeight={height}>
    {#if tile && size}
      {#key tile}
        <Tile {tile} {size} />
      {/key}
    {/if}
  </div>
</div>

<style lang="scss">
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
    align-items: center;
  }
</style>
