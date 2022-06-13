<script setup>
  const props = defineProps({
    joke: String
  })

  const { joke } = toRefs(props)

  const emit = defineEmits(['closeModal'])

</script>

<template>
  <section class="modal rim" @click="emit('closeModal')">
    {{joke}}
  </section>
</template>