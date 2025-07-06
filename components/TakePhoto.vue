<template>
    <div class="relative">
        <video ref="video" autoplay playsinline class="rounded-md w-full"></video>
        <UButton
            @click="captureImage"
            icon="i-heroicons-camera"
            size="xl"
            :ui="{rounded: 'rounded-full'}"
            class="absolute bottom-4 left-1/2 -translate-x-1/2"
            aria-label = "Take Photo"
        />
        <canvas  ref="canvas" class="hidden" > </canvas>
    </div>
</template>

<script setup lang="ts">
    const video = ref<HTMLVideoElement | null> (null)
    const canvas = ref<HTMLCanvasElement | null> (null)
    let stream: MediaStream | null = null

    const emit = defineEmits(['image-captured'])

    const startCamera = async () => {
        try {
            stream = await navigator.mediaDevices.getUserMedia({  video: {
                facingMode: { exact: "environment" },
            },})
            if (video.value) {
                video.value.srcObject = stream
            }
        } catch (error) {
            console.error("Error accessing camera:", error)
        }
    }

    const captureImage  = () => {
        if (video.value && canvas.value) {
            const context = canvas.value.getContext('2d')
            if (context) {
                canvas.value.width = video.value.videoWidth
                canvas.value.height= video.value.videoHeight
                context.drawImage(video.value, 0,0, canvas.value.width, canvas.value.height)
                const imageURL = canvas.value.toDataURL('image/png')
                emit('image-captured', imageURL)
            }
        }
    }
    onMounted(() => {
        startCamera()
    })

    onBeforeMount(() => {
        if (stream) {
            stream.getTracks().forEach(track => track.stop())
        }
    })
</script>