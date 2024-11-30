<script setup>
import {defineEmits, ref} from 'vue';
import {useForm} from "vee-validate";
import axios from "axios";

const loading = ref(false);
const ordered = ref(false);
const emit = defineEmits(['order']);

const {errors, defineField, handleSubmit, resetForm} = useForm({
  validationSchema: {
    name: val => (isRequired(val) ? true : 'This is required'),
    email: val => (isEmail(val) ? true : 'Incorrect email address'),
    address: val => (isRequired(val) ? true : 'This is required'),
    agree: val => (isAgree(val) ? true : 'This is required'),
  },
});
const [name, nameAttrs] = defineField("name");
const [email, emailAttrs] = defineField("email");
const [address, addressAttrs] = defineField("address");
const [agree, agreeAttrs] = defineField("agree");

const onSubmit = handleSubmit(values => {
  loading.value = true;

  axios.post('https://httpbin.org/anything', {
    order: values,
  })
    .then(function (response) {
      if (response.status === 200) {
        loading.value = false;
        ordered.value = true;
        setTimeout(() => {
          emit('order');
          ordered.value = false;
          resetForm();
        }, 5000);
      }
    })
    .catch(function (error) {
      console.log(error);
    });
});

function isRequired(value) {
  return value && value.trim();
}

function isEmail(value) {
  return /^[^\s@]+@[^\s@]+\.[^\s@]+$/.test(value);
}

function isAgree(value) {
  return value;
}
</script>

<template>
  <div v-if="loading" class="spinner-border" role="status">
    <span class="visually-hidden">Loading...</span>
  </div>
  <div v-if="!loading">
    <div v-if="ordered">
      Thank you for your order!
    </div>
    <div v-if="!ordered">
      <h3>Make an order</h3>
      <form @submit="onSubmit">
        <div class="form-group">
          <label for="name">Name</label>
          <input type="text" id="name" class="form-control" v-model="name" v-bind="nameAttrs" placeholder="Name"/>
          <span>{{ errors.name }}</span>
        </div>
        <div class="form-group">
          <label for="price">Email</label>
          <input type="email" id="email" class="form-control" v-model="email" v-bind="emailAttrs" placeholder="Email"/>
          <span>{{ errors.email }}</span>
        </div>
        <div class="form-group">
          <label for="address">Address</label>
          <input type="text" id="address" class="form-control" v-model="address" v-bind="addressAttrs"
                 placeholder="Address"/>
          <span>{{ errors.address }}</span>
        </div>
        <div class="form-check">
          <input type="checkbox" class="form-check-input" id="agree" v-model="agree" v-bind="agreeAttrs">
          <label class="form-check-label" for="agree">Agree with the terms</label>
          <span>{{ errors.agree }}</span>
        </div>
        <button class="btn btn-primary mt-2 mb-2">Order</button>
      </form>
    </div>
  </div>
</template>

<style scoped>

</style>