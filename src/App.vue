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
    // マップオブジェクト生成
    // データソースはOpenStreetMap
    const map = L.map("app").addLayer(
      L.tileLayer("https://{s}.tile.osm.org/{z}/{x}/{y}.png")
    );

    // 位置情報検索
    map.locate({ setView: true, maxZoom: 10 });

    // 位置情報取得成功処理
    function onLocationFound(e) {
      var radius = e.accuracy / 2;
      L.marker(e.latlng)
        .addTo(map)
        .bindPopup("You are within " + radius + " meters from this point")
        .openPopup();

      L.circle(e.latlng, radius).addTo(map);
    }
    map.on("locationfound", onLocationFound);

    // 位置情報取得エラー処理
    function onLocationError(e) {
      alert(e.message);
    }
    map.on("locationerror", onLocationError);
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