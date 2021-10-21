<template>
  <div class="h-100">
    <router-link to="/">Mapへもどる</router-link>
    <iframe id="ar_frame" class="h-100 w-100" src="ar.html" />
  </div>
</template>
<script lang="ts">
import { defineComponent, onMounted, ref, watch } from "vue";
import { useRoute } from "vue-router";

export default defineComponent({
  setup() {
    const route = useRoute();
    const lat = ref("");
    const lng = ref("");
    onMounted(() => {
      lat.value = route.query.lat as string;
      lng.value = route.query.lng as string;
      watch(
        () => [lat, lng],
        () => {
          console.log(`lat:${lat.value}, lng:${lng.value}`);
          const frame = document.querySelector(
            "#ar_frame"
          ) as HTMLIFrameElement;
          frame.contentWindow?.addEventListener("load", () => {
            frame.contentWindow?.postMessage(
              { lat: lat.value, lng: lng.value },
              "*"
            );
            console.log("passed");
          });
        },
        { immediate: true }
      );
    });
    return {};
  },
});
</script>
