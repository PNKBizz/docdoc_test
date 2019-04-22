<template>
  <form
    class="step-2 row"
    novalidate
    @submit.prevent="onSubmit"
  >
    <div class="col-12 radios">
      <div class="form-check">
        <label
          class="form-check-label"
        >
          <input
            v-model="deliveryMethod"
            class="form-check-input"
            type="radio"
            name="exampleRadios"
            value="courier"
          >
          Доставка
        </label>
      </div>
      <div class="form-check">
        <label
          class="form-check-label"
        >
          <input
            v-model="deliveryMethod"
            class="form-check-input"
            type="radio"
            name="exampleRadios"
            value="pickup"
          >
          Самовывоз
        </label>
      </div>
    </div>
    <template v-if="isCourier">
      <custom-input
        v-model="country"
        label="Страна"
        placeholder="Россия"
        class="col-sm-12 col-md-4"
        :error="errors.country"
        required
        @input="resetError('country')"
      />
      <custom-input
        v-model="city"
        label="Город"
        placeholder="Москва"
        class="col-sm-12 col-md-4"
        :error="errors.city"
        required
        @input="resetError('city')"
      />
      <custom-input
        v-model="zip"
        label="Индекс"
        type="number"
        placeholder="398000"
        class="col-sm-12 col-md-4"
        :error="errors.zip"
        required
        @input="resetError('zip')"
      />
      <custom-input
        v-model="address"
        label="Адрес"
        placeholder="г. Москва, ул. Космонавтов, 14/5"
        class="col-12"
        :error="errors.address"
        required
        @input="resetError('address')"
      />
      <custom-input
        v-model="date"
        label="Дата доставки"
        placeholder="24/05/2017"
        class="col-12"
        :error="errors.date"
        required
        @input="resetError('date')"
      />
    </template>
    <div class="form-group col-12 d-flex flex-column">
      <label>
        <b>Комментарий к заказу</b>
        <textarea
          v-model="comment"
          placeholder="Ваш комментарий здесь..."
          class="form-control textarea"
          rows="3"
        />
      </label>
    </div>
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
  country: '',
  city: '',
  zip: '',
  address: '',
  date: '',
});

export default {
  name: 'Step2',
  components: {
    CustomInput,
  },
  data() {
    return {
      country: '',
      city: '',
      zip: '',
      address: '',
      date: '',
      comment: '',
      errors: createEmptyErrors(),
      deliveryMethod: 'courier',
    };
  },
  computed: {
    isCourier() {
      return this.deliveryMethod === 'courier';
    },
  },
  methods: {
    validate() {
      if (this.isCourier) {
        Object.keys(this.errors).forEach((field) => {
          if (!this[field]) this.errors[field] = 'Поле обязательно для заполнения';
        });
        if (this.city && this.city.length > 255) this.errors.city = 'Длина названия города не более 255 символов';
        if (this.zip && this.zip.length > 6) this.errors.zip = 'Длина индекса не более 6 символов';
        if (this.address && this.address.length > 255) this.errors.address = 'Длина адреса не более 255 символов';
        if (this.date && /^((0|1)\d{1})\/((0|1|2)\d{1})\/((19|20)\d{2})/.test(this.date)) this.errors.date = 'Введите дату в формате ДД/ММ/ГГГГ';
      }
      return Object.keys(this.errors).every(_ => !this.errors[_]);
    },
    onSubmit() {
      if (this.validate()) {
        const data = {
          country: this.country,
          city: this.city,
          zip: this.zip,
          address: this.address,
          date: this.date,
          comment: this.comment,
        };
        this.$emit('finish', data);
      }
    },
    resetError(field) {
      this.errors[field] = '';
    },
  },
};
</script>

<style lang="scss" scoped>
.step-2 {
  padding: 30px 0;
}

.radios {
  margin-bottom: 10px;
}

.textarea {
  margin-top: 5px;
}
</style>
