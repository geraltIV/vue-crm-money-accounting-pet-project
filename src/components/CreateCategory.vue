<template>
  <div class="col s12 m6">
    <div>
      <div class="page-subtitle">
        <h4>Создать</h4>
      </div>

      <form @submit.prevent="handleSumbit">
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
          Создать
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
  name: "CreateCategory",
  data() {
    return {
      name: "",
      limit: 10,
    };
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
  mounted() {
    M.updateTextFields();
  },
  methods: {
    async handleSumbit() {
      if (this.$v.$invalid) {
        this.$v.$touch();
        return;
      }

      try {
        const category = await this.$store.dispatch("createCategory", {
          title: this.name,
          limit: this.limit,
        })

        console.log(category);
        this.name = ''
        this.limit = 10
        this.$v.$reset()
        this.$message('Категория создана')

        this.$emit('created', category)
      } catch (error) {
        console.log(error);
      }
    },
  },
};
</script>

<style>
</style>