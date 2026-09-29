<template>
  <div>
    <h1>QR Scanner Page</h1>
    
    <v-card>
      <v-card-text>
        <!-- Show qr scanner -->
        <video class="qr-video" ref="videoRef"></video>

        <!-- Show scanned result -->
        <p>Result: {{ result }}</p>

        <!-- Start and Stop Scanner -->
        <v-btn color="green" @click="startScanner">Start Scanner</v-btn>
        <v-btn color="red">Stop Scanner</v-btn>
      </v-card-text>
    </v-card>
  </div>
</template>

<script lang="ts" setup>
//@ts-nocheck

import QrScanner from 'qr-scanner'

definePageMeta({
  middleware: ['auth'],
})

const result = ref('')
const videoRef = ref<HTMLVideoElement | null>(null)
let scanner: QrScanner | null = null



const startScanner = async () => {


  if (!videoRef.value) return

  

  scanner = new QrScanner(
    videoRef.value,
    (scanResult) => {
      result.value = scanResult.data
      console.log('Scanned result:', scanResult.data)
    },
    {
      preferredCamera: 'environment',
      highlightScanRegion: true,
      highlightCodeOutline: true,
    }
  )

  await scanner.start()
}
  //Function to stop
  
  const stopScanner = () => {
    scanner?.destroy ()
    scanner = null
  }

onBeforeUnmount(() => {
  stopScanner()
})
</script>

<style>
.scanner-page {
  min-height: calc(100vh - 64px);
}

.qr-video {
  width: 100%;
  max-width: 520px;
  height: 360px;
  display: block;
  border: 1px solid #ccc;
  border-radius: 16px;
  background-color: #000;
  object-fit: cover;
}

</style>
Compose
