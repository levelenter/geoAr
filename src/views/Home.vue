<template>
  <div class="home mt-3">
    <p>map</p>
    <p>マップをクリックして物体を配置してください。</p>
  </div>

  <div class="border border-secondary rounded m-3">
    <div id="map" style="height: 30rem" class="w-100"></div>
  </div>
</template>

<script lang="ts">
/* eslint-disable no-undef */
import { defineComponent, onMounted } from "vue";
import { Loader } from "@googlemaps/js-api-loader";
import { useRouter } from "vue-router";

export default defineComponent({
  setup: () => {
    const router = useRouter();
    const loader = new Loader({
      apiKey: "AIzaSyB9D412riHCmBqpyiNRTMCZWfTcK1EUyHM",
      version: "weekly",
    });
    loader.load().then(() => {
      const geocoder = new google.maps.Geocoder();

      geocoder.geocode({ address: "東京都千代田区" }, (results, status) => {
        if (status == "OK" && results![0].geometry?.location !== undefined) {
          console.log(results![0].geometry?.location);
        }
      });
    });

    function addMarker(place: any, title: any, map: any) {
      let iconUrl = "http://maps.google.com/mapfiles/ms/icons/blue-dot.png";
      let marker = new google.maps.Marker({
        position: place,
        map: map,
        animation: google.maps.Animation.DROP,
        title: title,
        icon: iconUrl,
      });
      return marker;
    }

    function initMap() {
      navigator.geolocation.getCurrentPosition((pos) => {
        const myLatlng = {
          lat: pos.coords.latitude,
          lng: pos.coords.longitude,
        };
        const map = new google.maps.Map(document.getElementById("map")!, {
          zoom: 20,
          center: myLatlng,
        });

        /**
         * mapをクリックした時
         */
        map.addListener("click", (e: any) => {
          let latlng = e.latLng;
          console.log(latlng);
          let marker = addMarker(
            latlng,
            `${latlng.latitude},${latlng.longitude}`,
            map
          );
          map.panTo(marker.getPosition()!);
          console.log({ lat: latlng.lat(), lng: latlng.lng() });
          router.push({
            name: "GeoAR",
            query: { lat: latlng.lat(), lng: latlng.lng() },
          });

          /**
           * マーカーをクリックした時
           */
          marker.addListener("click", function () {
            map.setZoom(50);
            const pos = marker.getPosition();
            console.log("pos", pos);
            if (!pos) return;
            router.push({
              name: "GeoAR",
              query: { lat: pos.lat(), lng: pos.lng() },
            });
          });
        });
      });
    }

    onMounted(() => {
      initMap();
    });
  },
});
</script>
