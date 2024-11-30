<template>
    <div class="col">
        <div class="card text-center">
            <img :src="props.product.image" class="card-img-top" :alt="props.product.title">
            <div class="card-body">
                <h5 class="card-title">{{ props.product.title }}</h5>
                <p class="card-text">{{ props.product.description }}</p>
                <p class="card-text">Price: ${{ props.product.price }}</p>
                <button @click="$emit('addToCart', props.product.id)" class="btn btn-primary">Add to cart</button>
            </div>
        </div>
    </div>
</template>

<script setup>
const props = defineProps({
    product: Object,
});
</script>

<style scoped>
.card {
    width:18rem;
}
</style>