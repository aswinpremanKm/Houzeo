<template>
  <div>
    <form @submit.prevent="submitForm" class="form">
      <div v-for="(form, index) in forms" :key="index" class="form-group">
        <div class="form-control">
          <label for="name">Name</label> 
          <input v-model="form.name" type="text" id="name" class="input-text" required />
        </div>
        <div class="form-control">
          <label for="email">Email</label>
          <input v-model="form.email" type="email" id="email" class="input-text" required />
        </div>
        <div class="form-control">
          <label for="mobile">Mobile Number</label>
          <input v-model="form.mobile" type="number" id="mobile" class="input-text" placeholder="(844) 448-0110" required />
        </div>
        <div class="form-control">
          <label for="birthDate">Birth Date</label>
          <input v-model="form.birthDate" type="date" id="birthDate" class="input-text" required />
        </div>
        <div class="form-control">
          <label>Gender</label>
          <div class="radio-group">
            <label><input type="radio" v-model="form.gender" value="Male" /> Male</label>
            <label><input type="radio" v-model="form.gender" value="Female" /> Female</label>
            <label><input type="radio" v-model="form.gender" value="Other" /> Other</label>
          </div>
        </div>
        <div class="form-control">
          <label>Languages</label>
          <div class="checkbox-group">
            <label><input type="checkbox" v-model="form.languages" value="Angular" /> Angular</label>
            <label><input type="checkbox" v-model="form.languages" value="Vuejs" /> Vuejs</label>
            <label><input type="checkbox" v-model="form.languages" value="React.Js" /> React.Js</label>
          </div>
        </div>
        <div class="form-control">
          <label for="city">City</label>
          <select v-model="form.cities"  id="city" class="input-text">
            <option v-for="city in cities" :key="city" :value="city">{{ city }}</option>
          </select>
        </div>
        <div class="form-control">
          <label for="file">File Upload</label>
          <input type="file" @change="handleFileUpload($event, index)" id="file" accept=".jpeg,.pdf" class="input-file" required />
        </div>
        <button type="button" @click="removeForm(index)" v-if='index>0'  class="btn-remove">Remove</button>
      </div>
      <button type="button" @click="addForm" class="btn-add">+ Add More</button>
      <button type="submit" class="btn-submit">Submit</button>
      <p v-if="apiError" class="error-text">{{ apiError }}</p>
    </form>
  </div>
</template>

<script>
import { ref } from 'vue';

export default {
  props: {
    // Props definition if any
  },
  setup(props, { emit }) {
    const forms = ref([
      {
        name: '',
        email: '',
        mobile: '',
        birthDate: '',
        gender: '',
        languages: [],
        cities: [],
        file: null
      }
    ]);
    const cities = ['Delhi', 'Mumbai', 'Kochi', 'Bengaluru', 'Goa', 'Chennai', 'Bhopal'];
    const apiError = ref('');

    //Functionality to Add new group field
    const addForm = () => {
      forms.value.push({
        name: '',
        email: '',
        mobile: '',
        birthDate: '',
        gender: '',
        languages: [],
        cities: [],
        file: null
      });
    };

    //Functionality to remove existing field
    const removeForm = (index) => {
      forms.value.splice(index, 1);
    };

    //File Uploader
    const handleFileUpload = (event, index) => {
      const file = event.target.files[0];
        forms.value[index].file = file;
    };

   //Functionality to submit form and populate table
    const submitForm = async () => {
      const token = process.env.VUE_APP_JWT_TOKEN; 
      const formData = new FormData();

      forms.value.forEach((form, index) => {
        formData.append(`forms[${index}][name]`, form.name);
        formData.append(`forms[${index}][email]`, form.email);
        formData.append(`forms[${index}][mobile]`, form.mobile);
        formData.append(`forms[${index}][birthDate]`, form.birthDate);
        formData.append(`forms[${index}][gender]`, form.gender);
        formData.append(`forms[${index}][languages]`, form.languages);
        formData.append(`forms[${index}][cities]`, form.cities);
        if (form.file) {
          formData.append(`forms[${index}][file]`, form.file);
        }
      });

      try {
        const response = await fetch('https://reqres.in/api/users', {
          method: 'POST',
          headers: {
            'Authorization': `Bearer ${token}`
          },
          body: formData
        });

        if (!response.ok) {
          throw new Error('API error');
        }

        const data = await response.json();
        emit('formSubmitted', forms.value);
        forms.value=[{
           name: '',
        email: '',
        mobile: '',
        birthDate: '',
        gender: '',
        languages: [],
        cities: [],
        file: null
        }]
        console.log('Form submitted:', data);
      } catch (error) {
        apiError.value = error.message;
      }
    };

    return {
      forms,
      cities,
      apiError,
      addForm,
      removeForm,
      handleFileUpload,
      submitForm
    };
  }
};
</script>

<style scoped>


.form {
  max-width: 600px;
  margin: 0 auto;
  padding: 20px;
  border: 1px solid #ccc;
  border-radius: 8px;
  
}

.form-group {
  margin-bottom: 20px;
}

.form-control {
  margin-bottom: 10px;
}

label {
  display: block;
  font-weight: bold;
  margin-bottom: 5px;
}

.input-text,
.input-file {
  width: 100%;
  padding: 8px;
  border: 1px solid #ccc;
  border-radius: 4px;
}



.radio-group {
  margin-top: 5px;
}

.radio-group label {
  margin-right: 10px;
}

.checkbox-group {
  margin-top: 5px;
}

.checkbox-group label {
  display: block;
  margin-bottom: 5px;
}

.btn-submit,
.btn-add {
  background-color: #007bff;
  color: #fff;
  border: none;
  padding: 10px 20px;
  border-radius: 4px;
  cursor: pointer;
  margin-right: 10px;
}


.btn-remove {
  background-color: #dc3545;
  color: #fff;
  border: none;
  padding: 5px 10px;
  border-radius: 4px;
  cursor: pointer;
  margin-left: 10px;
}



.error-text {
  color: #dc3545;
  margin-top: 10px;
  
}
</style>
