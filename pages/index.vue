<template>
    <nav class="bg-white shadow-sm border-b border-gray-200">
        <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8">
            <div class="flex justify-between items-center h-16">
            <div class="flex items-center space-x-4">
                <UIcon name="i-heroicons-home" class="w-6 h-6 text-primary-600" />
                <h1 class="text-xl font-semibold text-gray-900">DocScan</h1>
            </div>
            <div class="flex items-center space-x-4">
                <UButton variant="ghost" size="sm">
                <UIcon name="i-heroicons-user" class="w-4 h-4 mr-2" />
                Profile
                </UButton>
                <UButton variant="ghost" size="sm">
                <UIcon name="i-heroicons-cog-6-tooth" class="w-4 h-4 mr-2" />
                Settings
                </UButton>
            </div>
            </div>
        </div>
    </nav>
    <!-- Main Content -->
        
    <UContainer class="py-8">
        <div class="max-w-2xl mx-auto">
        <h1 class="text-3xl font-bold text-center mb-2">Nuxt UI Photo Booth</h1>
        <p class="text-center text-gray-500 dark:text-gray-400 mb-8">
            Capture a photo and see it appear in the gallery below.
        </p>

        <TakePhoto @image-captured="addImageToGallery" class="mb-8" />

        <UDivider label="Your Gallery" class="mb-8" />

        <div v-if="pictures.length > 0">
            <UCarousel
            v-slot="{ item }"
            :items="pictures"
            :ui="{ item: 'basis-full md:basis-1/2' }"
            arrows
            class="rounded-lg overflow-hidden"
            >
            <img :src="item" class="w-full h-auto object-cover" draggable="false" alt="Captured gallery image">
            </UCarousel>
        </div>

        <div v-else class="text-center bg-gray-100 dark:bg-gray-800/50 p-8 rounded-lg">
            <UIcon name="i-heroicons-photo" class="text-4xl text-gray-400 dark:text-gray-500 mx-auto" />
            <p class="mt-2 text-sm text-gray-500 dark:text-gray-400">
            Your captured pictures will appear here.
            </p>
        </div>
        </div>
    </UContainer>
</template>


<script setup lang="ts">
    // This array will hold the data URLs of all captured pictures
    const pictures = ref<string[]>([])

    // This function is called when the TakePhoto component emits the 'image-captured' event
    const addImageToGallery = (imageUrl: string) => {
    // Add the new image to the beginning of the array to show it first
    pictures.value.unshift(imageUrl)
    }
</script>