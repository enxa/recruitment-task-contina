<script setup>
  const props = defineProps({
    joke: String
  })

  const { joke } = toRefs(props)

  const emit = defineEmits(['closeModal'])

</script>

<template>
  <section class="modal rim" @click="emit('closeModal')">
    <div class="rim">
      <h5>{{joke}}</h5>
      <button>✕</button>
    </div>
  </section>
</template>

<style scoped>
  .modal {
    position: fixed;
    top: 0;
    left: 0;
    width: 100%;
    min-height: 100vh;
    padding: 10vh 10vw;
    display: grid;
    place-items: center;
    text-align: center;
    background: rgba(237, 245, 245, 0.1);
    backdrop-filter: blur(10px);
  }
    .modal div {
      border-radius: .5rem;
      background: var(--color-aqua-haze);
      box-shadow:  -20px 20px 60px #c9d0d0,
                    20px -20px 60px #ffffff;
    }

      .modal button {
        background: transparent;
        font-size: var(--font-2);
        cursor: pointer;
      }
</style>