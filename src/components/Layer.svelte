<script lang="ts">
  import type { VectorTileLayer } from "@mapbox/vector-tile";
  import Feature from "./Feature.svelte";

  let { layer, color }: { layer: VectorTileLayer; color: string } = $props();
</script>

<g data-layer-name={layer.name}>
  {#each { length: layer.length } as _, i}
    <Feature feature={layer.feature(i)} {color} />
  {/each}
</g>
