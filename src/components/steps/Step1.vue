<template>
  <form
    class="step-1 row"
    novalidate
    @submit.prevent="onSubmit"
  >
    <custom-input
      v-model="firstName"
      label="Имя"
      placeholder="Иван"
      class="col-sm-12 col-md-6"
      :error="errors.firstName"
      required
      @input="resetError('firstName')"
    />
    <custom-input
      v-model="lastName"
      label="Фамилия"
      placeholder="Иванов"
      class="col-sm-12 col-md-6"
      :error="errors.lastName"
      required
      @input="resetError('lastName')"
    />
    <custom-input
      v-model="phone"
      label="Телефон"
      type="tel"
      placeholder="+79046524783"
      class="col-12"
      :error="errors.phone"
      required
      @input="resetError('phone')"
    />
    <custom-input
      v-model="email"
      label="Email"
      type="email"
      placeholder="example@example.com"
      class="col-12"
      :error="errors.email"
      required
      @input="resetError('email')"
    />
    <button
      class="btn btn-primary ml-3"
      type="submit"
    >
      Продолжить
    </button>
  </form>
</template>

<script>
import CustomInput from '@/components/form/CustomInput.vue';

const createEmptyErrors = () => ({
  firstName: '',
  lastName: '',
  phone: '',
  email: '',
});

export default {
  name: 'Step1',
  components: {
    CustomInput,
  },
  props: {
    data: Object,
  },
  data() {
    return {
      firstName: this.data.firstName,
      lastName: this.data.lastName,
      phone: this.data.phone,
      email: this.data.email,
      errors: createEmptyErrors(),
    };
  },
  methods: {
    validate() {
      Object.keys(this.errors).forEach((field) => {
        if (!this[field]) this.errors[field] = 'Поле обязательно для заполнения';
      });
      if (this.firstName && this.firstName.length > 255) this.errors.firstName = 'Длина имени не более 255 символов';
      if (this.lastName && this.lastName.length > 255) this.errors.lastName = 'Длина фамилии не более 255 символов';
      if (this.phone && !/^((\+7|7|8)+([0-9]){10})$/.test(this.phone)) this.errors.phone = 'Некорректный формат телефона';
      if (this.email && !/[^@]+@[^\.]+\..+/i.test(this.email)) this.errors.email = 'Некорректный email';
      return Object.keys(this.errors).every(_ => !this.errors[_]);
    },
    onSubmit() {
      if (this.validate()) {
        const data = {
          firstName: this.firstName,
          lastName: this.lastName,
          phone: this.phone,
          email: this.email,
        };
        this.$emit('submit', data);
      }
    },
    resetError(field) {
      this.errors[field] = '';
    },
  },
};
</script>

<style lang="scss" scoped>
.step-1 {
  padding: 30px 0;
}
</style>
