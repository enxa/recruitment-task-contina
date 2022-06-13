<script setup>
  const categorySelect = ref(null)
  const categories = ref([])
  const joke = ref({})

  const fetchCategory = async (url) => {
    const response = await fetch(url)
    const result = await response.json()
    categories.value = result
  }

  const fetchJoke = async (url) => {
    const response = await fetch(url)
    const result = await response.json()
    joke.value = result
  }

  onMounted(() => fetchCategory(`https://api.chucknorris.io/jokes/categories`))
  watchEffect(() => fetchJoke(`https://api.chucknorris.io/jokes/random?category=${categorySelect.value}`))

  const selectCategory = (category) => categorySelect.value = category
</script>

<template>
  <section class="categories rim">
      <div v-for="category in categories">
        <div @click="selectCategory(category)">{{category}}</div>
      </div>
  </section>

  <Modal :joke="joke.value" />
</template>