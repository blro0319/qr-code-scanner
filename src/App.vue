<script setup lang="ts">
import type { Html5QrcodeResult } from "html5-qrcode";
import QRCodeScanner from "./components/QRCodeScanner.vue";

function handleScan(result: Html5QrcodeResult) {
  alert(result.decodedText);
}
</script>

<template>
  <QRCodeScanner @scan="handleScan" />
</template>

<style scoped></style>
