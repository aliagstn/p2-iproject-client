<script>
import { mapState, mapActions } from 'pinia'
import { useMainStore } from '../stores/main'
export default {
    props: ["product"],
    computed: {
        ...mapState(useMainStore, ["hasLoggedIn"])
    },
    methods: {
        goToDetailPage: function () {
            // this.$router.push(`/detail/${this.el.id}`)
        },
        convertPrice: function () {
            this.price = new Intl.NumberFormat("id-ID", {
                style: "currency",
                currency: "IDR",
            }).format(this.product.price * 15000);
        },
        goToDetailPage: function () {
            this.$router.push(`/detail/${this.product.id}`)
        },
        changeImage: function () {
            this.image = "https://cdn-icons-png.flaticon.com/512/5856/5856842.png"
        },
    },
    created: function () {
        this.convertPrice()
        this.image = this.product.image_link
    },
    data() {
        return {
            price: 0,
            image: ""
        }
    }
}
</script>
<template>
    <div class="col">
        <div type="button" @click.prevent="goToDetailPage">
            <div class="card h-100 card-product text-center">
                <img :src="image" class="card-img-top rounded-circle" :alt="product.name"
                    style="max-width: 250px;max-height:250px;margin-left: 5px;" v-on:error="changeImage" />
                <div class="card-body">
                    <h5 class="card-title" style="font-family: 'Righteous', cursive;">{{ product.name }}</h5>
                    <h6 class="card-title"><small class="text-muted">{{ product.brand }}</small></h6>

                    <p class="card-text" style="font-family: 'Changa', sans-serif;">{{ price }}</p>
                </div>
            </div>
        </div>
    </div>
</template>