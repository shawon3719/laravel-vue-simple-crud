<template>
    <div>
        <!-- Page Heading -->
        <h1 class="h3 mb-2 text-gray-800">Tables</h1>
        <p class="mb-4">DataTables is a third party plugin that is used to generate the demo table below.
            For more information about DataTables, please visit the <a target="_blank"
                href="https://datatables.net">official DataTables documentation</a>.</p>

        <!-- DataTales Example -->
        <div class="card shadow mb-4">
            <div class="card-header py-3">
                <h6 class="m-0 font-weight-bold text-primary">DataTables Example</h6>
            </div>
            <div class="card-body">
                <div class="table-responsive">
                    <table class="table table-bordered" id="dataTable" width="100%" cellspacing="0">
                        <thead>
                            <tr>
                                <th>ID</th>
                                <th>Name</th>
                                <th>Detail</th>
                                <th>Actions</th>
                            </tr>
                        </thead>
                        <tfoot>
                            <tr>
                                <th>ID</th>
                                <th>Name</th>
                                <th>Detail</th>
                                <th>Actions</th>
                            </tr>
                        </tfoot>
                        <tbody>
                            <tr v-for="product in products" :key="product.id">
                                <td>{{ product.id }}</td>
                                <td>{{ product.name }}</td>
                                <td>{{ product.detail }}</td>
                                <td>
                                    <div class="btn-group" role="group">
                                        <router-link :to="{name: 'edit-product', params: { id: product.id }}" class="btn btn-success">Edit</router-link>
                                        <button class="btn btn-danger" @click="deleteProduct(product.id)">Delete</button>
                                    </div>
                                </td>
                            </tr>
                        </tbody>
                    </table>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
import axios from "axios";
import * as notify from "../../utils/notify.js";
import "datatables.net-dt/js/dataTables.dataTables"
import "datatables.net-dt/css/jquery.dataTables.min.css"

export default {
    name: "all-products",
    data() {
        return {
            products: []
        }
    },
    created() {
        axios.get('http://127.0.0.1/api/products')
        .then(response => {
            this.products = response.data;
            $('#dataTable').DataTable();
        });
    },
    methods: {
        async deleteProduct(id) {
        this.isLoading = true;
        try {
             axios.delete(`http://127.0.0.1/api/products/${id}`)
                .then(response => {
                let i = this.products.map(data => data.id).indexOf(id);
                this.products.splice(i, 1)
            });
        }catch (error) {
            notify.authError(error);
            this.isLoading = false;
        }
        },
    },
};
</script>