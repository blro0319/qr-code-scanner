<script setup lang="ts">
import { Html5Qrcode, type Html5QrcodeResult } from "html5-qrcode";
import { ref } from "vue";

const emit = defineEmits<{
  scan: [result: Html5QrcodeResult];
}>();

const whileScanning = ref(false);
const reader = ref<HTMLDivElement>();
let scanner: Html5Qrcode | null = null;

function toggleScan() {
  if (whileScanning.value) stopScan();
  else startScan();
}

function startScan() {
  if (!reader.value) return;
  if (!scanner) scanner = new Html5Qrcode("reader");

  scanner.start(
    { facingMode: "environment" },
    {
      fps: 5,
      aspectRatio: reader.value.clientWidth / reader.value.clientHeight,
    },
    (_, result) => emit("scan", result),
    () => {}
  );
  whileScanning.value = true;
}
function stopScan() {
  scanner?.stop();
  whileScanning.value = false;
}
</script>

<template>
  <div class="camera-area" :data-scanning="whileScanning">
    <div ref="reader" id="reader" />
    <div class="overlay">
      <div class="overlay__dim">
        <div class="overlay__dim__item" />
        <div class="overlay__dim__item" />
        <div class="overlay__dim__item" />
        <div class="overlay__dim__item" />
      </div>
      <div class="overlay__lines">
        <div class="overlay__lines__item" />
        <div class="overlay__lines__item" />
        <div class="overlay__lines__item" />
        <div class="overlay__lines__item" />
      </div>
    </div>
    <div class="controls">
      <button
        class="start-button"
        :aria-label="whileScanning ? '스캔 중지' : '스캔 시작'"
        :data-scanning="whileScanning"
        @click="toggleScan()"
      />
    </div>
  </div>
</template>

<style lang="scss" scoped>
@use "sass:math";

.camera-area {
  position: relative;

  display: flex;
  align-items: center;
  justify-content: center;
}

#reader {
  width: 100%;
  height: 100%;
}

.overlay {
  $box-size: 240px;
  $border-width: 8px;
  $line-length: 60px;

  pointer-events: none;

  position: absolute;
  inset: 0;

  &__dim {
    &__item {
      position: absolute;
      background-color: rgba(0 0 0 / 48%);
    }
    &__item:nth-child(1) {
      top: 0;
      left: 0;
      width: 100%;
      height: calc(50% - #{math.div($box-size, 2)});
    }
    &__item:nth-child(2) {
      top: calc(50% - #{math.div($box-size, 2)});
      left: calc(50% + #{math.div($box-size, 2)});
      width: calc(50% - #{math.div($box-size, 2)});
      height: $box-size;
    }
    &__item:nth-child(3) {
      top: calc(50% + #{math.div($box-size, 2)});
      left: 0;
      width: 100%;
      height: calc(50% - #{math.div($box-size, 2)});
    }
    &__item:nth-child(4) {
      top: calc(50% - #{math.div($box-size, 2)});
      left: 0;
      width: calc(50% - #{math.div($box-size, 2)});
      height: $box-size;
    }
  }

  &__lines {
    position: absolute;
    top: 50%;
    left: 50%;

    width: $box-size + $border-width * 2;
    height: $box-size + $border-width * 2;

    transform: translate(-50%, -50%);

    &__item {
      position: absolute;
      width: $line-length + $border-width;
      height: $line-length + $border-width;
      border: $border-width solid #e9e9f1;
    }
    &__item:nth-child(1) {
      top: 0;
      left: 0;
      border-right: none;
      border-bottom: none;
    }
    &__item:nth-child(2) {
      top: 0;
      right: 0;
      border-left: none;
      border-bottom: none;
    }
    &__item:nth-child(3) {
      bottom: 0;
      left: 0;
      border-right: none;
      border-top: none;
    }
    &__item:nth-child(4) {
      bottom: 0;
      right: 0;
      border-left: none;
      border-top: none;
    }
  }
}

.controls {
  position: absolute;
  bottom: 0;
  left: 0;

  display: flex;
  align-items: center;
  justify-content: center;

  width: 100%;
  height: 80px;

  background-color: rgba(0 0 0 / 24%);

  display: flex;
}
.start-button {
  display: flex;
  align-items: center;
  justify-content: center;

  width: 52px;
  height: 52px;

  background-color: #f4f4fa;
  border-radius: 26px;

  transition-property: background-color, outline-color, scale;
  transition-duration: 240ms;

  &:hover,
  &:focus-visible {
    background-color: #f4f4fa;
  }
  &:focus-visible {
    outline: 2px solid #ededf3;
    outline-offset: 2px;
  }
  &:active {
    scale: 96%;
  }

  &::before {
    content: "";

    display: block;
    width: 16px;
    height: 16px;

    background-color: #ff3738;
    border-radius: 16px;

    transition-property: background-color, border-radius;
    transition-duration: 240ms;
  }

  &[data-scanning="true"] {
    background-color: #ff3738;

    &:hover,
    &:focus-visible {
      background-color: #ff3738;
    }
    &:focus-visible {
      outline: 2px solid #cc2c2d;
      outline-offset: 2px;
    }
    &:active {
      scale: 96%;
    }

    &::before {
      border-radius: 2px;
      background-color: white;
    }
  }
}
</style>
