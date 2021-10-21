<template>
  <div class="h-100">
    <button class="btn btn-primary" @click="messagePass">pass</button>
    <iframe id="ar_frame" class="h-100 w-100" src="ar.html" />
  </div>
</template>
<script lang="ts">
import { defineComponent, ref } from "vue";
import { useRoute } from "vue-router";

export default defineComponent({
  setup() {
    const route = useRoute();
    const lat = ref(new Number(route.query.lat));
    const lng = ref(new Number(route.query.lng));
    const messagePass = () => {
      const frame = document.querySelector("#ar_frame") as HTMLIFrameElement;
      frame.contentWindow?.postMessage({ lat: lat.value, lng: lng.value }, "*");
      console.log("passed");
    };
    return {
      messagePass,
    };
  },
});
</script>
