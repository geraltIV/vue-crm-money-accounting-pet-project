<template>
  <div class="col s12 m6">
    <div>
      <div class="page-subtitle">
        <h4>Редактировать</h4>
      </div>

      <form @submit.prevent="handleSumbit">
        <div class="input-field">
          <select ref="select" v-model="current">
            <option
              v-for="category in categories"
              :key="category.id"
              :value="category.id"
            >
              {{ category.title }}
            </option>
          </select>
          <label>Выберите категорию</label>
        </div>

        <div class="input-field">
          <input
            id="name"
            type="text"
            v-model="name"
            :class="{
              invalid: $v.name.$dirty && !$v.name.required,
            }"
          />
          <label for="name">Название</label>
          <span
            class="helper-text invalid"
            v-if="$v.name.$dirty && !$v.name.required"
            >Введите название категории</span
          >
        </div>

        <div class="input-field">
          <input
            id="limit"
            type="number"
            v-model.number="limit"
            :class="{
              invalid:
                ($v.limit.$dirty && !$v.limit.required) ||
                ($v.limit.$dirty && !$v.limit.minValue),
            }"
          />
          <label for="limit">Лимит</label>
          <span
            class="helper-text invalid"
            v-if="$v.limit.$dirty && !$v.limit.minValue"
            >Минимальное значение {{ $v.limit.$params.minValue.min }}</span
          >
        </div>

        <button class="btn waves-effect waves-light" type="submit">
          Обновить
          <i class="material-icons right">send</i>
        </button>
      </form>
    </div>
  </div>
</template>

<script>
import { required, minValue } from "vuelidate/lib/validators";
import M from "materialize-css";

export default {
  name: "EditCategory",
  data() {
    return {
      select: null,
      name: "",
      limit: "",
      current: null,
    };
  },
  props: {
    categories: {
      type: Array,
      required: true,
      default() {
        return [];
      },
    },
  },
  validations: {
    name: {
      required,
    },
    limit: {
      required,
      minValue: minValue(10),
    },
  },
  watch: {
    current(catId) {
      const {title, limit} = this.categories.find(category => category.id === catId)
      this.name = title
      this.limit = limit
    },
  },
  created () {
    const {id, title, limit} = this.categories[0]
    this.current = id
    this.name = title
    this.limit = limit
  },
  mounted() {
    this.select = M.FormSelect.init(this.$refs.select, this.categories);
    M.updateTextFields();
  },
  destroyed() {
    if (this.select && this.select.destroy) this.select.destroy();
  },
  methods: {
    async handleSumbit() {
      if (this.$v.$invalid) {
        this.$v.$touch();
        return;
      }

      try {
        await this.$store.dispatch("updateCategory", {
          id: this.current,
          title: this.name,
          limit: this.limit,
        });

        this.$message("Категория обновлена");

        this.$emit("updated", {
          id: this.current,
          title: this.name,
          limit: this.limit,
        });
      } catch (error) {
        console.log(error);
      }
    },
  },
};
</script>

<style>
</style>