<template>
  <core-dialog
    :id="dialogId"
    :dialog-class="{ 'modal-sm': true }"
    :title="title"
  >
    <canvas id="canvas" width="300px" height="300px"></canvas>
  </core-dialog>
</template>
<script lang='ts'>
import { defineComponent, onMounted, ref } from "@vue/runtime-core";
import CoreDialog from "./CoreDialog.vue";
import { DialogHandlerCore } from "./DialogHandlerCore";
import { DialogIds } from "./DialogIds";
import qrcode from "qrcode";
export default defineComponent({
  components: { CoreDialog },
  props: {},
  setup: () => {
    const dialogId = DialogIds.qrdialog;
    const title = "QR Code";
    onMounted(() => {
      DialogHandlerCore.onOpenDialog(dialogId, (e: Event) => {
        const canvas = document.getElementById("canvas");
        qrcode.toCanvas(canvas, location.href, function (error: any) {
          if (error) console.error(error);
          console.log("success!");
        });
      });
    });
    return {
      title,
      dialogId,
    };
  },
});
</script>