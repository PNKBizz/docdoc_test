<template>
  <div
    id="app"
    class="container"
  >
    <nav>
      <ul class="nav nav-tabs">
        <li class="nav-item">
          <a
            :class="['nav-link', { active: isFirstStep } ]"
            href="#"
            @click.prevent="currentComponent = 'Step1'"
          >
            Основные данные
          </a>
        </li>
        <li class="nav-item">
          <a
            :class="['nav-link', isFirstStep ? 'disabled' : 'active' ]"
            href="#"
            :aria-disabled="isFirstStep"
          >
            Адрес доставки
          </a>
        </li>
      </ul>
    </nav>
    <component
      :is="currentComponent"
      :data="formData"
      @submit="onSubmit"
      @finish="onFinish"
    />
  </div>
</template>

<script>
import Step1 from './components/steps/Step1.vue';
import Step2 from './components/steps/Step2.vue';

const createEmptyForm = () => ({
  firstName: '',
  lastName: '',
  phone: '',
  email: '',
});

export default {
  name: 'App',
  components: {
    Step1,
    Step2,
  },
  data: () => ({
    currentComponent: 'Step1',
    formData: createEmptyForm(),
  }),
  computed: {
    isFirstStep() {
      return this.currentComponent === 'Step1';
    },
  },
  methods: {
    onSubmit(data) {
      this.formData = data;
      this.currentComponent = 'Step2';
    },
    async onFinish() {
      try {
        await fetch('test.php');
        console.log('yeah');
      } catch (error) {
        console.error(error);
      }
    },
  },
};
</script>

<style>
#app {
  margin: 60px auto;
}
</style>
