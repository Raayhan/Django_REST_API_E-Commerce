<template>
    <div class="page-category">
        <div class="columns is-multiline">
            <div class="column is-12">
                <div class="is-size-2 has-text-centered">
                    {{ category.name }}
                </div>
            </div>
           
            <ProductBox v-for="product in category.products" v-bind:key="product.id" v-bind:product="product"></ProductBox>
        </div>

    </div>
</template>

<script>
import axios from 'axios'
import { toast } from 'bulma-toast'
import ProductBox from '@/components/ProductBox.vue'

export default {
    name: 'Category',
    data() {
        return {
            category: {
                products: []
            }
        }
    },
    components: {
        ProductBox
    },
    mounted() {
        this.getCategory()
    },
    watch: {
        $route(to, from) {
            if (to.name === 'Category') {
            this.getCategory()
        }
      }
    },
    methods: {
        async getCategory() {
            const category_slug = this.$route.params.category_slug
            
            this.$store.commit('setIsLoading', true)
            await axios
                .get(`/api/v1/products/${category_slug}/`)
                .then(response => {
                    this.category = response.data
                    document.title = this.category.name + ' | Djackets'
                })
                .catch(error => {
                    console.log(error)
                    toast({
                        message: 'Something went wrong. Please try again.',
                        type: 'is-danger has-text-white',
                        dismissible: true,
                        pauseOnHover: true,
                        duration: 2000,
                        position: 'bottom-right',
                    })
                })
            this.$store.commit('setIsLoading', false)


        }
    }

}
</script>
