<script setup lang="ts">
import type { Html5QrcodeResult } from "html5-qrcode";
import QRCodeScanner from "./components/QRCodeScanner.vue";

function handleScan(result: Html5QrcodeResult) {
  alert(result.decodedText);
}
</script>

<template>
  <div class="main-frame">
    <QRCodeScanner class="camera-area" @scan="handleScan" />
  </div>
</template>

<style lang="scss" scoped>
$max-width: 1280px;

.main-frame {
  overflow: hidden;

  display: flex;
  flex-direction: column;

  width: 100%;
  height: 100%;
}

.camera-area {
  flex: 1 1 0;
  width: 100%;
  height: 0;

  background-color: black;
}

@media screen and (min-width: ($max-width + 1px)) {
  .main-frame {
    max-width: min($max-width, calc(100% - 64px));
    max-height: calc(100% - 64px);
    margin: auto;

    border-radius: 16px;
  }
}
</style>
