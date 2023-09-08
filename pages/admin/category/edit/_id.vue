<template>
    <div class="content-wrapper">
        <section class="content-header">
            <div class="container-fluid">
            </div>
        </section>

        <section class="content">
            <div class="card card-outline card-info">
                <div class="card-header">
                    <h3 class="card-title"><i class="nav-icon fas fa-folder"></i> EDIT CATEGORY</h3>
                    <div class="card-tools">
                        <button type="button" class="btn btn-tool" data-card-widget="collapse" title="Collapse">
                            <i class="fas fa-minus"></i>
                        </button>
                        <button type="button" class="btn btn-tool" data-card-widget="remove" title="Remove">
                            <i class="fas fa-times"></i>
                        </button>
                    </div>
                </div>
                <div class="card-body">
                    <form @submit.prevent="updateCategory">
                        <div class="form-group">
                            <label>GAMBAR</label>
                            <input type="file" @change="handleFileChange" class="form-control">
                        </div>
                        <div class="form-group">
                            <label>NAMA CATEGORY</label>
                            <input type="text" v-model="category.name" placeholder="Masukkan Nama Category" class="form-control">
                            <div class="mt-2" v-if="validation.name">
                            <b-alert show variant="danger">{{ validation.name[0] }}</b-alert>
                            </div>
                        </div>
                        <button class="btn btn-info mr-1 btn-submit" type="submit"><i class="fa fa-paper-plane"></i>
                         UPDATE</button>
                        <button class="btn btn-warning btn-reset" type="reset"><i class="fa fa-redo"></i>
                         DELETE</button>
                    </form>
                </div>
            </div>
        </section>

    </div>
</template>

<script>

export default {
    layout: 'admin',

    head() {
        return {
            title: 'Edit Category - MasKahfi.com - Belajar Sampai Pintar'
        }
    },

    data(){
        return {
            category: {
                image: '',
                name: ''
            },
            validation: []
        }
    },

    mounted(){
        this.$axios.get(`/api/admin/categories/${this.$route.params.id}`)
        .then(response => {
            this.category.name = response.data.data.name
        })
    },

    methods: {
        handleFileChange(e){
            let image = this.category.image = e.target.files[0]

            if(!image.type.match('image.*')){
                e.target.value = ''

                this.category.image = null

                this.$swal.fire({
                    title: 'Oops!',
                    text: 'Format File Tidak di Dukung!',
                    icon: 'error',
                    showConfirmButton: false,
                    timer: 2000
                })
            }
        },
        async updateCategory(){
            let formData = new FormData();

            formData.append('image', this.category.image)
            formData.append('name', this.category.name)
            formData.append("_method", "PATCH")

            await this.$axios.post(`/api/admin/categories/${this.$route.params.id}`, formData)
            .then(() => {
                this.$swal.fire({
                    title: 'BERHASIL!',
                    text: "Data Berhasil di Update",
                    icon: 'success',
                    showConfirmButton: false,
                    timer: 2000
                })

                this.$router.push({
                    name: 'admin-category'
                })
            })
            .catch(error => {
                this.validation = error.response.data
            })
            }
        },
}

</script>