<script setup lang="ts">
import { onMounted } from 'vue'

onMounted(async () => {
  const video = document.querySelector('video') || document.createElement('video')
  const constraints = {
    audio: true,
    video: true,
  }

  console.log(navigator,
    navigator.mediaDevices)

  try {
    const stream = await navigator.mediaDevices.getUserMedia(constraints)

    const videoTracks = stream.getVideoTracks()
    console.log('Got stream with constraints:', stream)
    console.log(`Using video device: ${videoTracks[0].label}`)
    stream.onremovetrack = () => {
      console.log('Stream ended')
    }
    video.srcObject = stream
    console.log(stream.getAudioTracks())
  }

  catch (error: unknown) {
    if (error.name === 'OverconstrainedError') {
      console.error(
        `The resolution ${constraints.video.width.exact}x${constraints.video.height.exact} px is not supported by your device.`,
      )
    }
    else if (error.name === 'NotAllowedError') {
      console.error(
        'You need to grant this page permission to access your camera and microphone.',
      )
    }
    else {
      console.error(`getUserMedia error: ${error.name}`, error)
    }
  }
})
</script>

<template>
  <video id="viedo" muted autoplay playsinline />
</template>

<style>
video {
  border: #900 solid 1px;
  width: 300px;
  height: 400px;
}
</style>
