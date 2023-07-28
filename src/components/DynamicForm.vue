<template>
  <div>
    <form @submit.prevent="handleSubmit" class="container mt-4">
      <div v-for="(field, index) in formFields" :key="index" class="form-group">
        <label :for="field.token">{{ field.props.title }}</label>
        <template v-if="field.type === 'text'">
          <input
            v-model="formData[field.token]"
            :id="field.token"
            class="form-control"
            :placeholder="field.props.placeholder"
          />
        </template>
        <template v-else-if="field.type === 'checkbox'">
          <input
            type="checkbox"
            v-model="formData[field.token]"
            :id="field.token"
            class="form-check-input"
          />
        </template>
        <template v-else-if="field.type === 'date'">
          <input
            type="date"
            v-model="formData[field.token]"
            :id="field.token"
            class="form-control"
            :placeholder="field.props.placeholder"
          />
        </template>
      </div>
      <button type="submit" class="btn btn-primary">Submit</button>
    </form>
  </div>
</template>

<script>
export default {
  data() {
    return {
      formFields: [],
      formData: {},
    };
  },
  props: {
    jsonData: {
      type: Object,
      required: true,
    },
  },
  created() {
    this.formFields = this.jsonData.blocks;
    this.formFields.forEach((field) => {
      this.formData[field.token] = field.props.default || "";
    });
  },
  methods: {
    handleSubmit() {
      if (this.validateForm()) {
        console.log("Form submitted:", this.formData);
      } else {
        console.log("Form validation failed.");
      }
    },
    validateForm() {
      for (const field of this.formFields) {
        if (field.props.required && !this.formData[field.token]) {
          return false;
        }

        if (
          field.props.required === "IS_PERSON_MINOR" &&
          !this.formData[field.token]
        ) {
          return false;
        }
      }
      return true;
    },
  },
};
</script>
