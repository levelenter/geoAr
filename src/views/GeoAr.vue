<template>
  <div class="h-100">
    <router-link to="/">Mapへもどる</router-link>
    <button class="btn btn-link" @click="showQr">QR</button>
    <iframe id="ar_frame" class="h-100 w-100" src="ar.html" />
    <qr-dialog />
  </div>
</template>
<script lang="ts">
import { DialogIds } from "@/components/dialog/DialogIds";
import { defineComponent, onMounted, ref, watch } from "vue";
import { useRoute } from "vue-router";
import { DialogHandlerCore } from "../components/dialog/DialogHandlerCore";
import QrDialog from "../components/dialog/QrDialog.vue";
export default defineComponent({
  components: { QrDialog },
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
    const showQr = () => {
      console.log(DialogIds.qrdialog);
      DialogHandlerCore.showDialog(DialogIds.qrdialog);
    };
    return { showQr };
  },
});
</script>
