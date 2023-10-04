<script lang="ts">
import {
  Html5QrcodeScanner,
  type Html5QrcodeCameraScanConfig,
  type Html5QrcodeResult,
} from "html5-qrcode";
import { onMounted } from "vue";

export interface Props extends Omit<Html5QrcodeCameraScanConfig, "fps"> {
  /**
   * @default
   * ```ts
   * 10
   * ```
   */
  fps?: number;
}

export interface Emits {
  (e: "scan", result: Html5QrcodeResult): void;
}
</script>

<script setup lang="ts">
const props = withDefaults(
  defineProps<
    Omit<Html5QrcodeCameraScanConfig, "fps"> & {
      /**
       * @default
       * ```ts
       * 10
       * ```
       */
      fps?: number;
    }
  >(),
  { fps: 10 }
);
const emit = defineEmits<Emits>();

const id = "qr-code-scanner";

onMounted(() => {
  const scanner = new Html5QrcodeScanner(id, { ...props }, undefined);
  scanner.render(
    (_, result) => emit("scan", result),
    (_, error) => console.error(error)
  );
});
</script>

<template>
  <div :id="id" />
</template>
