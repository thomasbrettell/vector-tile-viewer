<script lang="ts">
  import Pbf from "pbf";
  import { VectorTile } from "@mapbox/vector-tile";
  import Viewer from "./components/Viewer.svelte";
  import { onMount } from "svelte";

  let url = $state(
    "https://vector.openstreetmap.org/shortbread_v1/14/15073/9832.mvt"
  );

  let tile = $state<VectorTile | null>(null);

  const loadTile = async (url: string) => {
    const res = await fetch(url);
    const arrayBuffer = await res.arrayBuffer();

    const pbf = new Pbf(arrayBuffer);

    tile = new VectorTile(pbf);
  };

  const submitHandler = (e: SubmitEvent) => {
    e.preventDefault();

    loadTile(url);
  };

  onMount(() => {
    loadTile(url);
  });
</script>

<div class="app">
  <div class="info">
    <h1>Vector Tile Viewer</h1>
    <p>
      Enter a url of a vector tile file (.mvt, .pbf etc) to get a preview of
      what it looks like.
    </p>
    <form onsubmit={submitHandler}>
      <input bind:value={url} placeholder="enter url to mvt file" />
      <button>Enter</button>
    </form>
  </div>

  <Viewer {tile} />
</div>

<style lang="scss">
  .app {
    width: 100%;
    height: 100vh;
    display: flex;
    flex-direction: column;
  }

  h1 {
    margin: 0;
  }

  form {
    display: flex;
    max-width: 600px;
  }

  .info {
    padding: 1em;
  }

  input {
    width: 100%;
  }
</style>
