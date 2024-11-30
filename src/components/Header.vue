<template>
    <header>
        <div class="input-group">
            <input type="text" class="form-control" placeholder="Type product name or price" v-model.trim="search">
            <button type="button" class="btn btn-outline-primary" @click="$emit('search', search)">Search</button>
            <button type="button" class="btn btn-outline-danger" @click="$emit('search', ''); search = ''">Clear</button>
        </div>
    </header> 
</template>

<script setup>
import { ref } from 'vue';
const search = ref('');
</script>