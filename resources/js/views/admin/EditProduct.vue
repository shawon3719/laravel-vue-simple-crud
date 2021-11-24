<template>
    <div>
        <h3 class="text-center">Edit Product</h3>
        <div class="row">
            <div class="col-md-6">
                <form @submit.prevent="updateProduct">
                    <div class="form-group">
                        <label>Name</label>
                        <input type="text" class="form-control" v-model="product.name">
                    </div>
                    <div class="form-group">
                        <label>Detail</label>
                        <input type="text" class="form-control" v-model="product.detail">
                    </div>
                    <button type="submit" class="btn btn-primary">Update</button>
                </form>
            </div>
        </div>
    </div>
</template>
 
<script>
import axios from "axios";
import * as notify from "../../utils/notify.js";

export default {
    name: "edit-product",
    data() {
            return {
                product: {}
            }
        },
    created() {
        axios.get(`http://127.0.0.1/api/products/${this.$route.params.id}`)
        .then((res) => {
                    this.product = res.data;
                });
    },
    methods: {
        async updateProduct(id) {
        this.isLoading = true;
        try {
             axios.patch(`http://127.0.0.1/api/products/${this.$route.params.id}`, this.product)
                .then((res) => {
                        this.$router.push("/admin/pages/products");
                    });
        }catch (error) {
            notify.authError(error);
            this.isLoading = false;
        }
        },
    },
};
</script>