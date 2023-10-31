<template>
    <div>
        <div class="container">
            <products-filter
                v-model:sellerBox.check="seller"
                v-model:brandBox="brands"
                @filterBox="filterProducts"
                @dropFilters="clearFilters"
            />
            <products-cards v-if="filteredArray.length" :products-list="filteredArray" />
            <div v-else>Таких товарів немає</div>
        </div>
    </div>
</template>

<script>
import ProductsCards from './components/ProductsCards.vue'
import ProductsFilter from './components/ProductsFilter.vue'
import { productsArray } from './constants/StoreBase.js'

export default {
    name: 'App',
    components: { ProductsCards, ProductsFilter },
    data() {
        return {
            productsArray,
            filteredArray: productsArray,
            seller: [],
            brands: [],
            errorMessage: null,
        }
    },
    computed: {},
    methods: {
        filterProducts(sellers, brands) {
            if (sellers.length && brands.length) {
                this.filteredArray = this.productsArray.filter((product) => {
                    return sellers.includes(product.seller) && brands.includes(product.brand)
                })
            } else if (sellers.length) {
                this.filteredArray = this.productsArray.filter((product) => {
                    return sellers.includes(product.seller)
                })
            } else if (brands.length) {
                this.filteredArray = this.productsArray.filter((product) => {
                    return brands.includes(product.brand)
                })
            } else {
                this.filteredArray = this.productsArray
            }
        },

        clearFilters() {
            this.filteredArray = this.productsArray
            this.brands = []
            this.seller = []
        },
    },
}
</script>



<style lang="scss">
@import './assets/style/';
#app {
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    margin-block: 60px;
}
</style>
