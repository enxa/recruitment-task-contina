<script setup>
  const categorySelect = ref(null)
  const categories = ref([])
  const joke = ref({})
  const modalOpen = ref(false)
  const pending = ref(null)
  const error = ref(null)
  const sortNameAsc = ref(true)

  const fetchData = async (url) => {
    try {
      pending.value = true
      const response = await fetch(url)
      const result = await response.json()
      pending.value = false
      return result
    } catch(err) {
      error.value = err.message
    }
  }

  onMounted(async () => categories.value = await fetchData(`https://api.chucknorris.io/jokes/categories`))
  watchEffect(async () => joke.value = await fetchData(`https://api.chucknorris.io/jokes/random?category=${categorySelect.value}`))

  const selectCategory = (category) => {
    categorySelect.value = category
    modalOpen.value = true
  }

  const sortCategories = (sortBy) => {
    if (sortBy === 'name') {
      categories.value.reverse()
      sortNameAsc.value = !sortNameAsc.value
    }
    if (sortBy === 'random') {
      categories.value = categories.value
        .map(value => ({ value, sort: Math.random() }))
        .sort((a, b) => a.sort - b.sort)
        .map(({ value }) => value)
    }
  }
</script>

<template>
  <section class="categories rim">
    <button @click="sortCategories('name')">Sort by name {{sortNameAsc ? '▼' : '▲'}}</button>
    <button @click="sortCategories('random')">Sort randomly</button>

    <div v-for="category in categories">
      <div @click="selectCategory(category)">{{category}}</div>
    </div>
    <div v-if="pending">Loading...</div>
    <div v-else>    
      <div v-if="modalOpen">
        <Modal :joke="joke.value" @closeModal="modalOpen = false" />
      </div>
    </div>
    <div v-if="error">{{error}}</div>
  </section>
</template>