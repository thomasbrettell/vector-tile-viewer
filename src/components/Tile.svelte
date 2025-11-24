<script lang="ts">
  import { type VectorTile } from "@mapbox/vector-tile";
  import Layer from "./Layer.svelte";

  let { tile, size }: { tile: VectorTile; size: number } = $props();

  let extent = $state(4096);
  Object.entries(tile.layers).reduce((prev, [layerName, layer]) => {
    if (layer.extent > extent) extent = layer.extent;
    return prev;
  }, 4096);
</script>

<svg width={size} height={size} viewBox={`0 0 ${extent} ${extent}`}>
  {#each Object.values(tile.layers) as layer}
    <Layer {layer} />
  {/each}
</svg>
