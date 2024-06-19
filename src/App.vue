<template>
  <div class="container mx-auto p-4">
    <FormComponent @formSubmitted="handleFormSubmission"/>
    <TableComponent :forms="submittedForms"/>
  </div>
</template> 

<script>
import { ref } from 'vue';
import FormComponent from './components/FormComponent.vue';
import TableComponent from './components/TableComponent.vue';

export default {
  components: {
    FormComponent,
    TableComponent
  },
  setup() {
    const submittedForms = ref([]);

    const handleFormSubmission = (data) => {
      submittedForms.value = data;
    };

    return {
      submittedForms,
      handleFormSubmission
    };
  }
};
</script>
