<template>
  <div class="form-group d-flex flex-column">
    <label>
      {{ label }}
      <input
        :value="value"
        :type="type"
        :placeholder="placeholder"
        :required="required"
        :class="['form-control input', { 'is-invalid': error }]"
        @input="$emit('input', $event.target.value)"
        @change="$emit('change', $event.target.value)"
      >
    </label>
    <small
      v-if="error"
      class="form-text text-danger"
    >
      {{ error }}
    </small>
  </div>
</template>

<script>
export default {
  name: 'CustomInput',
  props: {
    value: String,
    label: String,
    type: {
      type: String,
      default: 'text',
    },
    error: String,
    placeholder: String,
    required: Boolean,
  },
};
</script>

<style lang="scss" scoped>
label {
  font-weight: bold;
}

.input {
  margin-top: 5px;
}
</style>
