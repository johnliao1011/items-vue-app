<template>
    <layout-container>
        <div class="container">
            <h2 class="text-center mt-5 mb-3">Items Overview</h2>
            <div class="card">
                <div class="card-header">
                    <router-link to="/create" class="btn btn-outline-primary">
                        Create Item
                    </router-link>
                </div>
            </div>
            <div class="card-body">
                <table class="table table-bordered">
                    <thead>
                        <tr>
                            <th>Title</th>
                            <th>Description</th>
                            <th width="250px">Action</th>
                        </tr>
                    </thead>
                    <tbody>
                        <tr v-for="item in items" :key="item.id">
                            <td>{{ item.title }}</td>
                            <td>{{ item.description }}</td>
                            <td>
                                <router-link :to = "`/edit/${item.id}`" class="btn btn-outline-info mx-1">Edit</router-link>
                                <router-link :to = "`/detail/${item.id}`" class="btn btn-outline-success mx-1">Detail</router-link>
                                <button @click="deleteItem(item.id)" class="btn btn-outline-danger mx-1">Delete</button>
                            </td>
                        </tr>
                    </tbody>
                </table>
            </div>
        </div>
    </layout-container>
</template>

<script>
import Swal from 'sweetalert2';
import LayoutContainer from '../LayoutContainer.vue';
import axios from 'axios';

    export default {
        name: 'ItemsOverview',
        component: {
            LayoutContainer
        },
        data() {
            return {
                items: []
            };
        },
        created() {
            this.getAllItems();
        },
        methods: {
            getAllItems() {
                axios.post('/api/allItems')
                .then(response => {
                    this.items = response.data;
                    return response;
                })
                .catch(error => {
                    return error;
                })
            },
            deleteItem(id){
                Swal.fire({
                    icon: 'warning',
                    title: 'Are you sure',
                    text: 'Item once deleted cannot be recovered!',
                    showCancelButton: true,
                    confirmButtonColor: '#3085de',
                    cancelButtonColor: '#d33',
                    confirmButtonText: 'Delete Item'
                }).then((result)=>{
                    if(result.isConfirmed){
                        axios.delete(`/api/items?id=${id}`)
                        .then(response=>{
                            Swal.fire({
                                icon: 'success',
                                title: 'Item Deleted Successfully',
                                showConfirmButton: false,
                                timer: 1500
                            })
                            this.getAllItems();
                            return response;
                        }).catch(error=>{
                            Swal.fire({
                                icon: 'error',
                                title: 'Some error occurred',
                                showConfirmButton: false,
                                timer: 1500
                            })
                            return error;
                        })
                    }
                })
            }
        }
    };
</script>