<template>
  <div
    @click.stop="copyMessage"
    class="container flex mx-auto my-4 transition transform rounded-full cursor-pointer hover:scale-105"
    :class="isUser ? 'bg-green-800' : 'bg-gray-700'"
  >
    <div>
      <img
        class="h-12 rounded-full"
        :src="message.userPhotoURL"
        :alt="`Avatar of ${message.userName}`"
      />
    </div>
    <div class="flex flex-grow">
      <div
        class="flex items-center justify-between flex-grow mx-4 text-xl font-thin text-purple-200"
      >
        <p>{{ message.text }}</p>
        <p class="text-sm italic">{{ message.userName }}</p>
      </div>
    </div>
  </div>
  <transition @leave="(el, done) => motions.notification.leave(done)">
    <div
      v-motion-pop="'notification'"
      class="fixed z-10 px-8 py-4 bg-yellow-300 rounded-full bottom-16 right-4"
      v-if="copied"
    >
      <p class="text-yellow-900">Message Copied!</p>
    </div>
  </transition>
</template>

<script setup>
  import { defineProps, computed, ref } from 'vue'
  import { useClipboard } from '@vueuse/core'
  import { useMotions } from '@vueuse/motion'

  import { authentication } from '~/helpers/useFirebase'

  const { user } = authentication()

  const isUser = computed(() => user.value.uid === props.message.userId)

  const props = defineProps({
    message: {
      type: Object,
      default: () => ({
        userName: '',
        userId: '',
        userPhotoURL: '',
        text: '',
        createdAt: '',
      }),
    },
  })

  const source = ref('')
  const { copy, copied } = useClipboard({ source })
  const copyMessage = () => {
    source.value = `${props.message.text} by ${props.message.userName}`
    copy()
  }

  const motions = useMotions()
</script>
