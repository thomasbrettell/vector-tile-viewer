<script lang="ts" module>
  const types: ("Unknown" | "Point" | "LineString" | "Polygon")[] = [
    "Unknown",
    "Point",
    "LineString",
    "Polygon",
  ];
</script>

<script lang="ts">
  import type { VectorTileFeature } from "@mapbox/vector-tile";

  let { feature, color }: { feature: VectorTileFeature; color: string } =
    $props();

  let type = types[feature.type];

  let geometries = feature.loadGeometry();
</script>

{#if type === "Polygon"}
  {#each geometries as geom}
    {@const d = geom.reduce((prev, curr, i) => {
      if (i === 0) {
        prev += `M ${curr.x},${curr.y}`;
      } else {
        prev += `L ${curr.x},${curr.y}`;
      }

      return prev;
    }, "")}

    <path fill={color} {d} />
  {/each}
{:else if type === "LineString"}
  {#each geometries as geom}
    {@const d = geom.reduce((prev, curr, i) => {
      if (i === 0) {
        prev += `M ${curr.x},${curr.y}`;
      } else {
        prev += `L ${curr.x},${curr.y}`;
      }

      return prev;
    }, "")}

    <path stroke={color} stroke-width={10} fill="none" {d} />
  {/each}
{:else if type === "Point"}
  {#each geometries as geom}
    {#each geom as point}
      <circle
        cx={point.x}
        cy={point.y}
        r="20"
        fill-opacity={0.35}
        stroke={color}
        fill={color}
        stroke-width={4}
      />
    {/each}
  {/each}
{/if}
