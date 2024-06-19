<template>
  <div v-if="forms.length">
    <table class="custom-table">
      <thead>
        <tr>
          <th>Name</th>
          <th>Email</th>
          <th>Mobile</th>
          <th>Birth Date</th>
          <th>Gender</th>
          <th>Languages</th>
          <th>City</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="(form, index) in forms" :key="index">
          <td>{{ form.name }}</td>
          <td>{{ form.email }}</td>
          <td>{{ form.mobile }}</td>
          <td>{{ form.birthDate }}</td>
          <td>{{ form.gender }}</td>
          <td>{{ form.languages.join(', ') }}</td>
          <td>{{ form.cities }}</td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script>
import { defineComponent } from 'vue';

export default defineComponent({
  props: {
    forms: {
      type: Array,
      required: true
    }
  }
});
</script>

<style scoped>

.custom-table {
  width: 100%;
  margin-top: 20px;
}

.custom-table th,
.custom-table td {
  border: 1px solid green;
  padding: 8px;
  text-align: left;
}

.custom-table th {
  font-weight: bold;
}


</style>
