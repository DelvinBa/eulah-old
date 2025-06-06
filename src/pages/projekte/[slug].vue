<template>
    <div class="min-h-screen bg-gradient-to-b from-gray-900 to-gray-800 text-gray-100">
        <!-- Back Link -->
        <NuxtLink to="/projekte"
            class="inline-flex items-center text-amber-400 hover:text-amber-300 font-medium ml-6 mt-6 transition-colors duration-300">
            <svg xmlns="http://www.w3.org/2000/svg" class="h-5 w-5 mr-2" fill="none" viewBox="0 0 24 24"
                stroke="currentColor">
                <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M10 19l-7-7m0 0l7-7m-7 7h18" />
            </svg>
            Zurück zu Projekten
        </NuxtLink>

        <div class="max-w-4xl mx-auto py-16 space-y-24 px-6">
            <!-- Title & Subtitle -->
            <header class="text-center">
                <h1
                    class="text-5xl font-bold mb-6 tracking-tight bg-clip-text text-transparent bg-gradient-to-r from-amber-300 to-amber-500">
                    {{ post.title }}</h1>
                <p class="text-xl text-gray-300 max-w-2xl mx-auto leading-relaxed">{{ post.subtitle }}</p>
            </header>

            <!-- Video -->
            <section v-if="post.video" class="flex justify-center">
                <div class="relative inline-block mx-auto max-w-3xl rounded-2xl overflow-hidden shadow-2xl group">
                    <video ref="videoPlayer" :src="post.video" autoplay loop playsinline :muted="isMuted"
                        class="w-auto max-w-full h-auto max-h-[80vh] rounded-2xl object-contain" />

                    <!-- Video Controls Overlay - Appears on Hover -->
                    <div
                        class="absolute inset-0 flex items-end justify-between opacity-0 group-hover:opacity-100 transition-opacity duration-300 bg-gradient-to-t from-black/70 to-transparent p-4">
                        <!-- Play/Pause Button -->
                        <button @click.prevent="togglePlay"
                            class="text-white hover:text-amber-400 transition-colors p-2">
                            <svg v-if="isPlaying" xmlns="http://www.w3.org/2000/svg" class="h-8 w-8" fill="none"
                                viewBox="0 0 24 24" stroke="currentColor">
                                <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2"
                                    d="M10 9v6m4-6v6m7-3a9 9 0 11-18 0 9 9 0 0118 0z" />
                            </svg>
                            <svg v-else xmlns="http://www.w3.org/2000/svg" class="h-8 w-8" fill="none"
                                viewBox="0 0 24 24" stroke="currentColor">
                                <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2"
                                    d="M14.752 11.168l-3.197-2.132A1 1 0 0010 9.87v4.263a1 1 0 001.555.832l3.197-2.132a1 1 0 000-1.664z" />
                                <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2"
                                    d="M21 12a9 9 0 11-18 0 9 9 0 0118 0z" />
                            </svg>
                        </button>

                        <!-- Audio Controls -->
                        <div class="flex items-center space-x-4">
                            <!-- Volume Slider -->
                            <div class="w-32 flex items-center space-x-2">
                                <input type="range" min="0" max="1" step="0.1" v-model="volume" @input="adjustVolume"
                                    class="w-full h-1 bg-gray-400 rounded-lg appearance-none cursor-pointer accent-amber-400" />
                            </div>

                            <!-- Mute Button -->
                            <button @click.prevent="toggleMute"
                                class="text-white hover:text-amber-400 transition-colors p-2">
                                <svg v-if="isMuted" xmlns="http://www.w3.org/2000/svg" class="h-6 w-6" fill="none"
                                    viewBox="0 0 24 24" stroke="currentColor">
                                    <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2"
                                        d="M5.586 15H4a1 1 0 01-1-1v-4a1 1 0 011-1h1.586l4.707-4.707C10.923 3.663 12 4.109 12 5v14c0 .891-1.077 1.337-1.707.707L5.586 15z"
                                        clip-rule="evenodd" />
                                    <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2"
                                        d="M17 14l2-2m0 0l2-2m-2 2l-2-2m2 2l2 2" />
                                </svg>
                                <svg v-else xmlns="http://www.w3.org/2000/svg" class="h-6 w-6" fill="none"
                                    viewBox="0 0 24 24" stroke="currentColor">
                                    <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2"
                                        d="M15.536 8.464a5 5 0 010 7.072m2.828-9.9a9 9 0 010 12.728M5.586 15H4a1 1 0 01-1-1v-4a1 1 0 011-1h1.586l4.707-4.707C10.923 3.663 12 4.109 12 5v14c0 .891-1.077 1.337-1.707.707L5.586 15z" />
                                </svg>
                            </button>
                        </div>
                    </div>

                    <!-- Play Button Overlay - Centered, Shows When Paused -->
                    <div v-if="!isPlaying" @click="togglePlay"
                        class="absolute inset-0 flex items-center justify-center bg-black/30 cursor-pointer">
                        <div class="rounded-full bg-amber-500/80 p-5">
                            <svg xmlns="http://www.w3.org/2000/svg" class="h-12 w-12 text-white" fill="currentColor"
                                viewBox="0 0 24 24">
                                <path d="M8 5v14l11-7z" />
                            </svg>
                        </div>
                    </div>
                </div>
            </section>

            <!-- Story -->
            <section class="relative">
                <div
                    class="absolute -top-10 left-1/2 transform -translate-x-1/2 w-40 h-1 bg-gradient-to-r from-transparent via-amber-400 to-transparent">
                </div>
                <h2 class="text-3xl font-semibold text-center mb-10 text-amber-200">The Journey</h2>
                <div class="prose prose-invert prose-lg text-gray-300 leading-relaxed" v-html="formattedStory">

                </div>
            </section>

            <!-- Gallery -->
            <section v-if="post.images?.length" class="relative">
                <div
                    class="absolute -top-10 left-1/2 transform -translate-x-1/2 w-40 h-1 bg-gradient-to-r from-transparent via-amber-400 to-transparent">
                </div>
                <h2 class="text-3xl font-semibold text-center mb-10 text-amber-200">Gallery</h2>
                <div class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-2 gap-6 max-w-6xl mx-auto">
                    <div v-for="(img, i) in post.images" :key="i"
                        class="cursor-pointer overflow-hidden rounded-xl shadow-lg transform transition-transform duration-300 hover:scale-105 hover:shadow-amber-500/30"
                        @click="openLightbox(i)">
                        <NuxtImg :src="img"
                            class="w-full h-auto object-cover transition-transform duration-500 hover:scale-110"
                            alt="Projekt Bild" />
                    </div>
                </div>
            </section>

            <!-- Numbers -->
            <section v-if="post.numbers" class="relative">
                <div
                    class="absolute -top-10 left-1/2 transform -translate-x-1/2 w-40 h-1 bg-gradient-to-r from-transparent via-amber-400 to-transparent">
                </div>
                <h2 class="text-3xl font-semibold text-center mb-10 text-amber-200">Let's Talk Numbers</h2>
                <div class="prose prose-invert prose-lg mx-auto max-w-prose bg-gray-800/50 p-8 rounded-xl shadow-inner">
                    <p class="text-gray-300 text-lg whitespace-pre-line leading-relaxed">{{ formattedNumbers }}</p>
                </div>
            </section>

            <!-- CTA -->
            <section class="text-center relative">
                <div
                    class="absolute -top-10 left-1/2 transform -translate-x-1/2 w-40 h-1 bg-gradient-to-r from-transparent via-amber-400 to-transparent">
                </div>
                <h2 class="text-3xl font-semibold mb-10 text-amber-200">Bereit für Ihr eigenes Projekt?</h2>
                <div class="flex flex-col sm:flex-row justify-center gap-6">
                    <button @click="scrollToFunnel"
                        class="px-8 py-4 bg-gradient-to-r from-amber-500 to-amber-600 text-gray-900 rounded-full font-medium transform transition-all duration-300 hover:scale-105 hover:shadow-lg hover:shadow-amber-500/30">
                        Jetzt beraten lassen
                    </button>
                    <button @click="scrollToFunnel"
                        class="px-8 py-4 border-2 border-amber-400 rounded-full font-medium text-amber-400 transform transition-all duration-300 hover:scale-105 hover:text-amber-300 hover:border-amber-300 hover:shadow-lg hover:shadow-amber-500/20">
                        Co-Founder finden
                    </button>
                </div>
            </section>

            <!-- Footer -->
            <footer class="text-center pt-16">
                <NuxtLink to="/projekte"
                    class="inline-flex items-center text-amber-400 hover:text-amber-300 transition-colors duration-300">
                    <svg xmlns="http://www.w3.org/2000/svg" class="h-5 w-5 mr-2" fill="none" viewBox="0 0 24 24"
                        stroke="currentColor">
                        <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2"
                            d="M10 19l-7-7m0 0l7-7m-7 7h18" />
                    </svg>
                    Zurück zu Projekten
                </NuxtLink>
            </footer>
        </div>

        <!-- Lightbox Overlay -->
        <transition name="fade">
            <div v-if="lightboxOpen" class="fixed inset-0 bg-black bg-opacity-95 z-50 flex items-center justify-center"
                @click.self="closeLightbox">
                <button @click="closeLightbox"
                    class="absolute top-8 right-8 text-white hover:text-amber-400 text-4xl transition-colors duration-300">
                    ×
                </button>
                <button @click.prevent="prevImage"
                    class="absolute left-8 text-white hover:text-amber-400 text-5xl transition-colors duration-300">
                    ‹
                </button>
                <div class="transition-transform duration-500 ease-in-out">
                    <NuxtImg :src="post.images[currentImageIndex]"
                        class="max-w-[90vw] max-h-[90vh] object-contain rounded-lg shadow-2xl" alt="Lightbox Bild" />
                </div>
                <button @click.prevent="nextImage"
                    class="absolute right-8 text-white hover:text-amber-400 text-5xl transition-colors duration-300">
                    ›
                </button>

                <!-- Image counter -->
                <div
                    class="absolute bottom-8 left-1/2 transform -translate-x-1/2 text-white bg-black/50 px-4 py-2 rounded-full">
                    {{ currentImageIndex + 1 }} / {{ post.images.length }}
                </div>
            </div>
        </transition>
    </div>
</template>

<script setup>

const route = useRoute()
const { data: post } = await useAsyncData(route.path, () =>
    queryCollection('projekte').path(route.path).first()
)

const formattedStory = computed(() => {
    if (!post.value?.story) return ''

    return post.value.story.replace(/\\n/g, '\n').replace(/\*\*(.*?)\*\*/g, '<strong>$1</strong>').replace(/\n/g, '<br>')
})

const formattedNumbers = computed(() =>
    post.value.numbers.replace(/\\n/g, '\n')
)
// Video Controls
const videoPlayer = ref(null)
const isMuted = ref(false)
const volume = ref(0.5)
const isPlaying = ref(true)

onMounted(() => {
    if (videoPlayer.value) {
        videoPlayer.value.muted = isMuted.value
        videoPlayer.value.volume = volume.value
        videoPlayer.value.addEventListener('play', () => (isPlaying.value = true))
        videoPlayer.value.addEventListener('pause', () => (isPlaying.value = false))
    }
    // Check if we haven't reloaded yet
    const hasReloaded = sessionStorage.getItem(`reloaded-${route.path}`)

    if (!hasReloaded && import.meta.client) {
        // Mark this page as reloaded
        sessionStorage.setItem(`reloaded-${route.path}`, 'true')
        // Reload the page
        window.location.reload()
    }
})


onBeforeUnmount(() => {
    if (import.meta.client) {
        // Remove the reload status for this page
        sessionStorage.removeItem(`reloaded-${route.path}`)
    }
})



const baseUrl = 'https://eulah.de'  // statisch

const canonicalUrl = `${baseUrl}${route.path}`
// SEO: Set meta tags using useHead once the post is loaded
useHead(() => ({
    title: post.value
        ? `${post.value.title} | Eulah Software Projekte`
        : 'Umgesetzte Projekte ',
    link: [
        {
            key: 'canonical',
            rel: 'canonical',
            href: canonicalUrl
        }
    ],
    meta: [
        {
            name: 'description',
            // You can combine post.description with author/date/etc. if you want:
            // e.g. `${post.value.description} – Von ${post.value.author}, veröffentlicht am ${formatDate(post.value.date)}`
            content: post.value
                ? post.value.description
                : 'Blog post by Eulah Software.'
        },
        {
            name: 'author',
            content: post.value
                ? post.value.author
                : ''
        },
        {
            property: 'og:title',
            content: post.value
                ? `${post.value.title} | Eulah Software Blog`
                : ''
        },
        {
            property: 'og:description',
            content: post.value
                ? post.value.description
                : ''
        },
        {
            property: 'og:type',
            content: 'article'
        },
        ...(post.value && post.value.image
            ? [{ property: 'og:image', content: post.value.image }]
            : [])
    ]
}))

function togglePlay() {
    if (!videoPlayer.value) return
    videoPlayer.value.paused ? videoPlayer.value.play() : videoPlayer.value.pause()
}

function toggleMute() {
    isMuted.value = !isMuted.value
    if (videoPlayer.value) {
        videoPlayer.value.muted = isMuted.value
        if (!isMuted.value) {
            if (videoPlayer.value.paused) videoPlayer.value.play()
        }
    }
}

function adjustVolume() {
    if (videoPlayer.value) {
        videoPlayer.value.volume = volume.value
        if (volume.value > 0) {
            isMuted.value = false
            videoPlayer.value.muted = false
        } else {
            isMuted.value = true
            videoPlayer.value.muted = true
        }
    }
}

// Gallery Lightbox
const lightboxOpen = ref(false)
const currentImageIndex = ref(0)

function openLightbox(i) {
    currentImageIndex.value = i
    lightboxOpen.value = true
    document.body.style.overflow = 'hidden'
}

function closeLightbox() {
    lightboxOpen.value = false
    document.body.style.overflow = ''
}

function prevImage() {
    currentImageIndex.value = (currentImageIndex.value - 1 + post.value.images.length) % post.value.images.length
}

function nextImage() {
    currentImageIndex.value = (currentImageIndex.value + 1) % post.value.images.length
}

onMounted(() => {
    window.addEventListener('keydown', handleKey)

    // Add click event to video to toggle play/pause
    if (videoPlayer.value) {
        videoPlayer.value.addEventListener('click', togglePlay)
    }
})

function handleKey(e) {
    if (!lightboxOpen.value) return
    if (e.key === 'Escape') closeLightbox()
    if (e.key === 'ArrowLeft') prevImage()
    if (e.key === 'ArrowRight') nextImage()
}

onBeforeUnmount(() => {
    window.removeEventListener('keydown', handleKey)
    if (videoPlayer.value) {
        videoPlayer.value.removeEventListener('click', togglePlay)
    }
})

// Scroll to funnel
function scrollToFunnel() {
    const funnel = document.getElementById('lead-funnel')
    if (funnel) funnel.scrollIntoView({ behavior: 'smooth' })
}
</script>

<style>
.prose-invert {
    --tw-prose-body: theme('colors.gray.300');
    --tw-prose-headings: theme('colors.gray.100');
    --tw-prose-links: theme('colors.amber.400');
    --tw-prose-bold: theme('colors.gray.100');
    --tw-prose-captions: theme('colors.gray.400');
    --tw-prose-code: theme('colors.gray.100');
    --tw-prose-pre-code: theme('colors.gray.100');
    --tw-prose-pre-bg: theme('colors.gray.800');
    --tw-prose-hr: theme('colors.amber.600');
}

/* Lightbox fade transition */
.fade-enter-active,
.fade-leave-active {
    transition: opacity 0.3s ease;
}

.fade-enter-from,
.fade-leave-to {
    opacity: 0;
}

/* Custom scrollbar for webkit browsers */
::-webkit-scrollbar {
    width: 8px;
}

::-webkit-scrollbar-track {
    background: rgba(31, 41, 55, 0.5);
}

::-webkit-scrollbar-thumb {
    background: rgba(217, 119, 6, 0.5);
    border-radius: 4px;
}

::-webkit-scrollbar-thumb:hover {
    background: rgba(245, 158, 11, 0.7);
}

/* Range input styling */
input[type="range"]::-webkit-slider-thumb {
    -webkit-appearance: none;
    appearance: none;
    width: 12px;
    height: 12px;
    border-radius: 50%;
    background: #f59e0b;
    cursor: pointer;
}

input[type="range"]::-moz-range-thumb {
    width: 12px;
    height: 12px;
    border-radius: 50%;
    background: #f59e0b;
    cursor: pointer;
    border: none;
}
</style>