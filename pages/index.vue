<script setup lang="ts">
const peerStore = usePeerStore()
const router = useRouter()

const roomId = ref('')
const isLoading = ref(false)

async function handleCreateRoom() {
  if (isLoading.value)
    return
  isLoading.value = true
  await peerStore.createPeer()
  isLoading.value = false
  router.push(`/room/${peerStore.peerId}`)
}

async function handleJoinRoom() {
  if (!roomId.value || isLoading.value)
    return
  isLoading.value = true
  await peerStore.createPeer()
  isLoading.value = false
  router.push(`/room/${roomId.value}`)
}
</script>

<template>
  <div class="h-dvh flex flex-col items-center justify-center bg-gray-50 bg-pattern p-4">
    <div class="flex flex-col items-center justify-center p-4 sm:p-10 shadow-lg bg-gray-700/20 rounded-lg backdrop-blur-sm backdrop-filter">
      <h1 class="font-black text-4xl sm:text-5xl mb-4 drop-shadow-lg text-white text-center">
        WebRTC
      </h1>

      <div class="flex flex-col gap-4 w-60 sm:w-80">
        <button
          class="flex items-center justify-center gap-2 px-4 py-2 bg-orange-300 rounded-lg font-bold text-white transition hover:(bg-orange-400) disabled:(cursor-not-allowed bg-gray-400)"
          :disabled="isLoading"
          @click="handleCreateRoom"
        >
          <div class="i-tabler-video-plus w-5 h-5 mt-0.5" />
          <span v-if="!isLoading">Create New Room</span>
          <span v-else>Creating...</span>
        </button>
        <div class="relative flex h-6 items-center gap-2">
          <hr class="w-full"><span class="text-white text-xl font-bold pb-1">or</span><hr class="w-full">
        </div>
        <form class="flex" @submit.prevent="handleJoinRoom">
          <input v-model.trim="roomId" type="text" placeholder="Enter Room ID" size="5" class="px-4 py-2 rounded-l-lg outline-none flex-1">
          <button
            class="flex items-center gap-2 px-4 py-2 bg-orange-300 transition hover:(bg-orange-400) rounded-r-lg font-bold text-white cursor-pointer disabled:(cursor-not-allowed bg-gray-400)"
            :disabled="isLoading"
            @click="handleJoinRoom"
          >
            <div class="i-tabler-door-enter h-5 w-5 mt-0.5" />
            <span>Join</span>
          </button>
        </form>
      </div>
    </div>
  </div>
</template>
