<script lang="ts">
  import { create } from 'd3';
  import { vectorBasemapLayer, VectorBasemapLayer } from 'esri-leaflet-vector'
  import * as L from 'leaflet';
  import "leaflet/dist/leaflet.css";

  import MapMarkers from './MapMarkers.svelte';


  let map: L.Map;
  const apiKey = "AAPK3dfaa40a13c0404983142c26b566596ammsJLVROPRkVaZnrwj6bYIrYdi4FEikx7NZpYg7f5M9XlV2RFL6PgxMA_56IceHv";

  function createMap(container) {
    const basemaps = {
    'Topographic': vectorBasemapLayer("ArcGIS:Topographic", {apiKey: apiKey}),
    'Imagery': vectorBasemapLayer("ArcGIS:Imagery", {apiKey: apiKey})
    }

    map = L.map(container, {preferCanvas: true}).setView([41.550000,-85.8000000], 17);
    // let bm = new VectorBasemapLayer("ArcGIS:Topographic", {apiKey: apiKey});

    basemaps['Imagery'].addTo(map);

    (new L.Control.Layers(basemaps)).addTo(map);
  }

  function mapAction(container) {
    const map = createMap(container);
    return {
      destroy: () => {
        map
      }
    };
  }
</script>

<div id='map' use:mapAction>this is the map</div>

{#if map}
<MapMarkers map={map}/>
{/if}


<style>
  #map {
    /* background-color: aquamarine; */
    position: fixed;
    top: 0;
    left: 0;
    z-index: 0;
    width: 100vw;
    height: 100vh;
  }

  :global(.leaflet-retina .leaflet-control-layers-toggle) {
    background-image: url(../layers-2x.png) !important;
  }

  :global(.leaflet-control-layers label) {
    font-size: 1.1rem !important;
  }

</style>

