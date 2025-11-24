<script lang="ts">
  import { type VectorTile } from "@mapbox/vector-tile";
  import Layer from "./Layer.svelte";
  import * as d3 from "d3";

  let {
    tile,
    size,
    color,
  }: {
    tile: VectorTile;
    size: number;
    color: d3.ScaleOrdinal<string, string, never>;
  } = $props();

  let extent = $state(4096);
  Object.entries(tile.layers).reduce((prev, [layerName, layer]) => {
    if (layer.extent > extent) extent = layer.extent;
    return prev;
  }, 4096);
</script>

<svg width={size} height={size} viewBox={`0 0 ${extent} ${extent}`}>
  {#each Object.entries(tile.layers) as [layerName, layer]}
    <Layer {layer} color={color(layerName)} />
  {/each}
</svg>
