<template>
    <div class="products-container">
        <div v-for="product in productsList" :key="product.id" class="product">
            <a :href="product.link" class="product-image">
                <img :src="product.image" />
            </a>
            <a :href="product.link">
                <h4 class="product-title">{{ product.name }}</h4>
            </a>
            <div class="product-price">{{ product.price }}$</div>
        </div>
    </div>
</template>

<script>
export default {
    name: 'ProductsCards',
    props: {
        productsList: {
            type: Array,
            required: true,
        },
    },
}
</script>

<style lang="scss" scoped>
</style>