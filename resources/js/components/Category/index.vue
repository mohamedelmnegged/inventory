<template>
    <div>
        <div class="row">
            <router-link to="/store-category" class="btn btn-primary">Add Category</router-link>

        </div>
        <br>
        <input class="form-control" v-model="searchTerm" style="width: 300px;" type="text" placeholder="Search Here">

        <div class="container-fluid" id="container-wrapper">
            <div class="d-sm-flex align-items-center justify-content-between mb-4">

            </div>

            <div class="row">
                <div class="col-lg-12 mb-4">
                    <!-- Simple Tables -->
                    <div class="card">
                        <div class="card-header py-3 d-flex flex-row align-items-center justify-content-between">
                            <h6 class="m-0 font-weight-bold text-primary">Categories</h6>
                        </div>
                        <div class="table-responsive">
                            <table class="table align-items-center table-flush">
                                <thead class="thead-light">
                                <tr>
                                    <th>Category Name</th>
                                    <th>Action</th>



                                </tr>
                                </thead>
                                <tbody>
                                <tr v-for="category in filterSearch" :key="category.id">
                                    <td>{{category.category_name}}</td>


                                    <td>
                                        <a @click="deletecategory(category.id)" class="btn btn-sm btn-danger">Delete</a>
                                        <router-link :to="{name:'edit-category', params:{id:category.id}}"
                                                     class="btn btn-sm btn-info">Edit
                                        </router-link>

                                    </td>
                                </tr>

                                </tbody>
                            </table>
                        </div>
                        <div class="card-footer"></div>
                    </div>
                </div>
            </div>
            <!--Row-->
        </div>


    </div>
</template>


<script type="text/javascript">

    export default {

        created() {
            if (!User.loggedIn()) {
                this.$router.push('home')
            }

            this.allCategories();

        },
        data() {
            return {
                categories: [],
                searchTerm: ''

            }

        },
        computed: {
            filterSearch() {
                return this.categories.filter(category => {
                    return category.category_name.match(this.searchTerm)

                })
            }
        },
        methods: {
            allCategories() {
                axios.get('/api/category/')
                    .then(({data}) => (this.categories = data))
                    .catch()

            },
            deletecategory(id) {
                Swal.fire({
                    title: 'Are you sure?',
                    text: "You won't be able to revert this!",
                    icon: 'warning',
                    showCancelButton: true,
                    confirmButtonColor: '#3085d6',
                    cancelButtonColor: '#d33',
                    confirmButtonText: 'Yes, delete it!'
                }).then((result) => {
                    if (result.isConfirmed) {
                        axios.delete('/api/category/' + id)
                            .then(() => {
                                this.categories = this.categories.filter(category => {
                                    return category.id != id;

                                })

                            })
                            .catch(() => {
                                this.$router.push({name: 'category'})
                            })
                        Swal.fire(
                            'Deleted!',
                            ' category deleted.',
                            'success'
                        )
                    }
                })


            }


        },


    }


</script>


<style type="text/css">
    #em_photo {
        height: 40px;
        width: 40px;
    }

    a.btn.btn-sm {
        color: white;
    }


</style>
