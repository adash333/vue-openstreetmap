<template>
  <div id="app"></div>
</template>

<script>
import "leaflet/dist/leaflet.css";
import L from "leaflet";

delete L.Icon.Default.prototype._getIconUrl;

L.Icon.Default.mergeOptions({
  iconUrl: require("leaflet/dist/images/marker-icon.png"),
  iconRetinaUrl: require("leaflet/dist/images/marker-icon-2x.png"),
  shadowUrl: require("leaflet/dist/images/marker-shadow.png")
});

export default {
  mounted() {
    const map = L.map("app", {
      center: L.latLng(35.6825, 139.752778),
      zoom: 15
    })
      .addLayer(L.tileLayer("https://{s}.tile.osm.org/{z}/{x}/{y}.png"))
      .on("click", p => map.addLayer(L.marker(p.latlng)));
  }
};
</script>

<style>
html,
body,
#app {
  height: 100%;
}
body {
  margin: 0;
}
</style>