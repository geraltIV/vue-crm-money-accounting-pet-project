<template>
  <div>
    <div class="page-title">
      <h3>Категории</h3>
    </div>
    <section>
      <Loader v-if="loading" />

      <div class="row" v-else>
        <CreateCategory @created="addNewCategory" />

        <EditCategory
          v-if="categories.length"
          :categories="categories"
          @updated="updateCategories"
          :key="categories.length + updateCount"
        />
        <p v-else class="center">Категорий пока не создано</p>
      </div>
    </section>
  </div>
</template>

<script>
import CreateCategory from "../components/CreateCategory";
import EditCategory from "../components/EditCategory";

export default {
  name: "Categories",
  data() {
    return {
      categories: [],
      loading: true,
      updateCount: 0,
    };
  },
  components: {
    CreateCategory,
    EditCategory,
  },
  async mounted() {
    this.categories = await this.$store.dispatch("getCategories");
    this.loading = false;
  },
  methods: {
    addNewCategory(category) {
      this.categories.push(category);
    },
    updateCategories(category) {
      const index = this.categories.findIndex((c) => c.id === category.id)
      this.categories[index].title = category.title
      this.categories[index].limit = category.limit
      this.updateCount++
    },
  },
};
</script>

<style>
</style>