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
    if (sortBy === 'nameAsc') {
      if (!categories.value.reduce((n, item) => n !== false && item >= n && item)) {
        categories.value.sort()
      }
      categories.value.reverse()
      sortNameAsc.value = !sortNameAsc.value
    }
    if (sortBy === 'nameDsc') {
      if (!categories.value.reduce((n, item) => n !== false && item >= n && item)) {
        categories.value.sort()
      }
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
    <div v-if="pending">Loading...</div>
    <div v-else>
      <div class="sortBtns">
        <button v-if="sortNameAsc" @click="sortCategories('nameAsc')"><h6>Sort by name '▲'</h6></button>
        <button v-if="!sortNameAsc" @click="sortCategories('nameDsc')"><h6>Sort by name '▼'</h6></button>
        <button @click="sortCategories('random')"><h6>Sort randomly</h6></button>
      </div>
      <div class="categoriesList">
        <div v-for="category in categories">
          <div class="category" @click="selectCategory(category)">{{category}}</div>
        </div>
      </div>
      <div v-if="modalOpen">
        <Modal :joke="joke.value" @closeModal="modalOpen = false" />
      </div>
    </div>

    <div v-if="error">{{error}}</div>
  </section>
</template>

<style scoped>
  .categories {
    width: 100%;
    min-height: 100vh;
    padding: 10vh 10vw;
    display: grid;
    place-items: center;
    text-align: center;
  }
    .categories .sortBtns {
      display: flex;
      flex-flow: row wrap;
      justify-content: center;
      gap: var(--font-3);
      padding-bottom: var(--font-3);
    }
      .categories button {
        background: transparent;
        cursor: pointer;
      }
        .categories button h6 {
          padding: 0;
        }

    .categories .categoriesList {
      display: flex;
      flex-flow: row wrap;
      gap: var(--font-1);
      justify-content: center;
      align-items: center;
    }

      .categories .category {
        cursor: pointer;
        padding: var(--font-1) var(--font-2);
        border-radius: .5rem;
        background: var(--color-aqua-haze);
        box-shadow:  -20px 20px 60px #c9d0d0,
                      20px -20px 60px #ffffff;
      }
</style>