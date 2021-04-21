<template>
  <div
    class="fixed top-0 left-0 right-0 z-10 py-6 bg-purple-900 text-coolGray-300"
  >
    <div class="container flex items-center justify-between mx-auto">
      <div class="flex space-x-2 text-3xl brand">
        <mdi:message-processing-outline class="text-yellow-300" />
        <h1 class="font-thin">
          Chit<span class="font-bold text-yellow-300">Chat</span>
        </h1>
      </div>
      <div class="menu">
        <div v-if="!isAuthenticated">
          <button
            class="px-4 py-2 text-white bg-green-700 rounded-full hover:bg-green-900 focus:outline-none focus:ring-4 focus:ring-green-500"
            @click="googlePopup"
          >
            Login
          </button>
        </div>
        <div v-else class="flex items-center space-x-8">
          <p>Welcome {{ user?.displayName }}</p>
          <button
            class="px-4 py-2 text-white bg-red-700 rounded-full hover:bg-red-900 focus:outline-none focus:ring-4 focus:ring-red-500"
            @click="signOut"
          >
            Logout
          </button>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
  import { authentication } from '~/helpers/useFirebase'

  import { shallowRef } from 'vue'

  const { isAuthenticated, user, googlePopup, signOut } = authentication()
</script>
