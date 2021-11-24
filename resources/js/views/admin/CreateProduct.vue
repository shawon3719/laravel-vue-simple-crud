<template>
    <div>
        <h3 class="text-center">Create Product</h3>
        <div class="row">
            <div class="col-md-6">
                <form @submit.prevent="addProduct">
                    <div class="form-group">
                        <label>Name</label>
                        <input type="text" class="form-control" v-model="product.name">
                    </div>
                    <div class="form-group">
                        <label>Detail</label>
                        <input type="text" class="form-control" v-model="product.detail">
                    </div>
                    <button type="submit" class="btn btn-primary">Create</button>
                </form>
            </div>
        </div>
    </div>
</template>
 
<script>

import axios from "axios";
import * as notify from "../../utils/notify.js";

export default {
  name: "add-product",
  data() {
            return {
                product: {}
            }
        },
  methods: {
    async addProduct() {
      this.isLoading = true;
      try {
        var response = await axios.post('http://127.0.0.1/api/products', this.product);
        this.$router.push("/admin/pages/products");

      }catch (error) {
        notify.authError(error);
        this.isLoading = false;
      }
    },
  },
};

</script>