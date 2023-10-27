<template>
    <div class="filters-container">
        <div class="filter">
            <ul class="filter-list" ref="sellerFilter">
                <h5 class="filter-title">Постачальники</h5>
                <li v-for="seller in sellerOptions" :key="seller" class="filter-item">
                    <label class="filter-label">
                        <input @click="handleSellerCheckbox(seller)" v-model="sellerCheckBox[seller]" type="checkbox" />
                        {{ seller }}
                    </label>
                </li>
            </ul>
        </div>
        <div class="search">
            <input type="text" placeholder="Пошук" v-model="searchFilterText" />
        </div>
        <div class="filter">
            <ul v-if="searchFilter.length" ref="brandFilter" class="filter-list">
                <h5 class="filter-title">Бренди</h5>
                <li v-for="brand in searchFilter" :key="brand" class="filter-item">
                    <label class="filter-label">
                        <input @click="handleBrandCheckbox(brand)" v-model="brandCheckBox[brand]" type="checkbox" />
                        {{ brand }}
                    </label>
                </li>
            </ul>
            <div v-else>Таких брендів немає</div>
        </div>
        <button @click="$emit('dropFilters')">Скинути фільтри</button>
    </div>
</template>

<script>
import { productsArray } from '../constants/StoreBase.js'

export default {
    name: 'ProductsFilter',
    props: {
        brandBox: {
            type: Array,
            required: true,
        },
        brandBoxModifiers: { default: () => ({}) },
        sellerBox: {
            type: Array,
            required: true,
        },
        sellerBoxModifiers: { default: () => ({}) },
    },

    data() {
        return {
            searchFilterText: null,
            selectedSellers: [],
            selectedBrands: [],
        }
    },

    computed: {
        brandOptions() {
            const brandsList = productsArray.map((product) => product.brand)
            return this.getNewList(brandsList)
        },
        sellerOptions() {
            const sellersList = productsArray.map((product) => product.seller)
            return this.getNewList(sellersList)
        },

        searchFilter() {
            if (!this.searchFilterText) {
                return this.brandOptions
            } else {
                const searchParam = this.searchFilterText.toLowerCase()
                return this.brandOptions.filter((brand) => {
                    return brand && brand.toLowerCase().includes(searchParam)
                })
            }
        },

        sellerCheckBox: {
            get() {
                return this.sellerBox
            },
            set(val) {
                if (this.sellerBoxModifiers.check) {
                    // Чомусь нічого не виводиться, якщо я у в-моделі "sellerCheckBox[seller]" забираю "[seller]", то "this.sellerBoxModifiers.check" працює, але тоді чекбокси по дивному себе поводять
                    console.log(this.sellerBox)
                    if (!this.sellerBox.length) this.$refs.sellerFilter.style.border = '1px solid green'
                    else this.$refs.sellerFilter.style.border = 'none'
                }
                this.$emit('update:sellerBox', val)
            },
        },
        brandCheckBox: {
            get() {
                return this.brandBox
            },
            set(val) {
                this.$emit('update:brandBox', val)
            },
        },
    },
    methods: {
        getNewList(list) {
            return [...new Set(list)].sort()
        },

        handleSellerCheckbox(seller) {
            if (this.selectedSellers.includes(seller)) {
                this.selectedSellers = this.selectedSellers.filter((item) => item !== seller)
            } else {
                this.selectedSellers.push(seller)
            }
            this.$emit('filterBox', this.selectedSellers, this.selectedBrands)
        },

        handleBrandCheckbox(brand) {
            if (this.selectedBrands.includes(brand)) {
                this.selectedBrands = this.selectedBrands.filter((item) => item !== brand)
            } else {
                this.selectedBrands.push(brand)
            }
            this.$emit('filterBox', this.selectedSellers, this.selectedBrands)
        },
    },
}
</script>