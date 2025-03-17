<template>
  <div id="map" class="map"></div>
</template>

<script setup>
import { onMounted } from "vue";
import Map from "ol/Map";
import View from "ol/View";
import { fromLonLat, transformExtent } from "ol/proj";
import TileLayer from "ol/layer/Tile";
import XYZ from "ol/source/XYZ";
import GeoTIFF from "ol/source/GeoTIFF";
import ImageLayer from "ol/layer/Image";

const extent = [
  2811378.395, 
  5369135.155, 
  3061378.395, 
  5619135.155   
];

const transformedExtent = transformExtent(extent, 'EPSG:32633', 'EPSG:3857');

onMounted(() => {
  const map = new Map({
    target: "map",
    layers: [
      new TileLayer({
        source: new XYZ({
          url: "https://tile.openstreetmap.org/{z}/{x}/{y}.png",
        }),
      }),
      new ImageLayer({
        source: new GeoTIFF({
          url: '/VMI.tiff',
          extent: transformedExtent
        }),
      }),
    ],
    view: new View({
      center: fromLonLat([12.6, 46.2]),
      zoom: 9.8,
    }),
  });
});
</script>

<style>
.map {
  width: 100%;
  height: 100vh;
  position: absolute;
  top: 0;
  left: 0;
}
</style>
