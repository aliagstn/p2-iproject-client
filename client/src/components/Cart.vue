<script>
import { mapActions, mapState } from "pinia";
import { useMainStore } from "../stores/main";
import TableItem from "./TableItem.vue";
export default {
    created: function () {
        this.getCart()
    },
    methods: {
        ...mapActions(useMainStore, ['getProductsInCart', 'paymentByMidtrans', 'deleteCart']),
        getCart: function () {
            this.getProductsInCart()
        },
        toPricing: function(totalPrice){
            // console.log(totalPrice)
            this.megaTotalPrice += totalPrice
            // console.log(this.megaTotalPrice)
        },
        converTotalPrice: function(){
            return new Intl.NumberFormat("id-ID", {
                style: "currency",
                currency: "IDR",
            }).format(this.megaTotalPrice);
        },
        checkingOut: function(){
            this.paymentByMidtrans(this.megaTotalPrice)
                .then(() => {
                    this.deleteCart()
                })
                .catch((err) => {
                    console.log(err)
                })
        }
    },
    computed: {
        ...mapState(useMainStore, ['cartProducts'])
    },
    components: {
        TableItem
    },
    data(){
        return{
            megaTotalPrice: 0
        }
    }
}
</script>
<template>
    <div class="modal-dialog modal-xl modal-dialog-centered forcart">
        <div class="modal-content">
            <div class="modal-header">
                <h5 class="modal-title" id="exampleModalLabel">Your Cart</h5>
                <button type="button" class="btn-close" data-bs-dismiss="modal" aria-label="Close"></button>
            </div>
            <div class="modal-body">
                <div class="container">
                    <div class="row">
                        <div class="col">
                            <table class="table">
                                <thead>
                                    <tr>
                                        <!-- <th scope="col-2"></th> -->
                                        <th scope="col-2">Brand</th>
                                        <th scope="col-4">Product</th>
                                        <th scope="col-3">Quantity</th>
                                        <th scope="col-2">Sub Total Price</th>
                                        <th scope="col-2"></th>
                                    </tr>
                                </thead>
                                <tbody class="align-middle">
                                    <TableItem v-for="(product, i) in cartProducts" :key="i" :product="product" @toPricing="toPricing" />

                                </tbody>
                            </table>
                        </div>
                    </div>
                </div>
            </div>
            <div class="modal-footer">
                <button type="button" class="btn btn-primary buttoncart form-control" @click.prevent="checkingOut">Checkout for <i>{{converTotalPrice()}}</i></button>
            </div>
        </div>
    </div>
</template>
<style>
.buttoncart{
    background-color: rgba(208, 226, 245, 0.5);
    color: black;
    border: none;
}
.forcart{
    font-family: 'Changa', sans-serif;
}
</style>