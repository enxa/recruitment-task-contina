<script setup>
  const props = defineProps({
    joke: String
  })

  const { joke } = toRefs(props)

  // watchEffect(async () => {
  //   const { data: joke, pending, error, refresh } = await useFetch(`https://api.chucknorris.io/jokes/random?category=${category.value}`)

  //   console.log(joke)
  // })

</script>

<template>
  <section class="modal rim">
    {{joke}}
  </section>
</template>