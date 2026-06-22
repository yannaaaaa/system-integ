<template>
  <div>
    <video
      ref="videoRef"
      autoplay
      playsinline
      muted
      style="
        width: 100%;
        max-width: 400px;
        border: 1px solid #ccc;
        border-radius: 8px;
      "
    />

    <div class="mt-4">
      <v-btn color="primary" @click="StartScanning">
        Start
      </v-btn>

      <v-btn class="ml-2" color="error" @click="StopScanning">
        Stop
      </v-btn>
    </div>

    <div v-if="qrResult" class="mt-4">
      <strong>Scanned QR Code:</strong>
      <div>{{ qrResult }}</div>
    </div>
  </div>
</template>

<script setup lang="ts">
import { ref, onMounted, onBeforeUnmount } from 'vue'

const videoRef = ref<HTMLVideoElement | null>(null)
const qrResult = ref('')

let scanner: any = null

onMounted(async () => {
  if (!videoRef.value) {
    console.error('Video element not found')
    return
  }

  const QrScanner = (await import('qr-scanner')).default

  scanner = new QrScanner(
    videoRef.value,
    (result: any) => {
      console.log('Scanned:', result.data)
      qrResult.value = result.data
    },
    {
      returnDetailedScanResult: true,
      highlightScanRegion: true,
      highlightCodeOutline: true,
    }
  )

  console.log('Scanner initialized')
})

const StartScanning = async () => {
  try {
    if (!scanner) {
      console.error('Scanner not initialized')
      return
    }

    await scanner.start()

    console.log('Camera started')
  } catch (err) {
    console.error('Failed to start scanner:', err)
  }
}

const StopScanning = () => {
  scanner?.stop()
  console.log('Camera stopped')
}

onBeforeUnmount(() => {
  scanner?.stop()
  scanner?.destroy()
})
</script>