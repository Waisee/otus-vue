<script setup>
import { defineEmits } from 'vue';
import { useForm } from "vee-validate";

  const emit = defineEmits(['submit']);

  const { errors, defineField, handleSubmit, resetForm } = useForm({
    validationSchema: {
      title: val => (isRequired(val) ? true : 'This is required'),
      price: val => (isRequiredNumber(val) ? true : 'This is required and must be a number'),
      description: val => (isRequired(val) ? true : 'This is required'),
      category: val => (isRequired(val) ? true : 'This is required'),
    },
  });
  const [title, titleAttrs] = defineField("title");
  const [price, priceAttrs] = defineField("price");
  const [description, descriptionAttrs] = defineField("description");
  const [category, categoryAttrs] = defineField("category");
  const [img, imgAttrs] = defineField("img");

  const onSubmit = handleSubmit(values => {
    values.id = Math.floor(Math.random() * 1000);
    emit('submit', values);
    resetForm();
  });

  function isRequired(value) {
    return value && value.trim();
  }

  function isRequiredNumber(value) {
    return value && typeof value === "number";
  }
</script>

<template>
  <h3>Add product</h3>
  <form @submit="onSubmit">
    <div class="form-group">
      <label for="title">Title</label>
      <input type="text" id="title" class="form-control" v-model="title" v-bind="titleAttrs" placeholder="Title" />
      <span>{{ errors.title }}</span>
    </div>
    <div class="form-group">
      <label for="price">Price</label>
      <input type="number" id="price" class="form-control" v-model="price" v-bind="priceAttrs" placeholder="Price" />
      <span>{{ errors.price }}</span>
    </div>
    <div class="form-group">
      <label for="description">Description</label>
      <input type="text" id="description" class="form-control" v-model="description" v-bind="descriptionAttrs" placeholder="Description" />
      <span>{{ errors.description }}</span>
    </div>
    <label for="category">Category</label>
    <div class="form-group">
      <input type="text" id="category" class="form-control" v-model="category" v-bind="categoryAttrs" placeholder="Category" />
      <span>{{ errors.category }}</span>
    </div>
    <label for="img">Image path</label>
    <div class="form-group">
      <input type="text" id="img" class="form-control" v-model="img" v-bind="imgAttrs" placeholder="Image path" />
    </div>
    <button class="btn btn-primary mt-2 mb-2">Add</button>
  </form>
</template>

<style scoped>

</style>