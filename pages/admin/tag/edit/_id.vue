<template>

<div class="content-wrapper">
    <section class="content-header">
        <div class="container-fluid">
        </div>
    </section>
    <section class="content">
        <div class="card card-outline card-info">
            <div class="card-header">
                <h3 class="card-title"><i class="nav-icon fas fa-tags"></i>EDIT TAG</h3>
                <div class="card-tools">
                    <button class="btn btn-tool" type="button" data-card-widget="collapse" title="Collapse">
                        <i class="fas fa-minus"></i>
                    </button>
                    <button class="btn btn-tool" data-card-widget="remove" title="Remove">
                        <i class="fas fa-times"></i>
                    </button>
                </div>
            </div>
            <div class="card-body">
                <form @submit.prevent="updateTag">
                    <div class="form-group">
                        <label>NAMA TAG</label>
                        <input type="text" v-model="tag" placeholder="Masukkan Nama Tag" class="form-control">
                        <div class="mt-2" v-if="validation.name">
                            <b-alert show variant="danger">{{ validation.name[0] }}</b-alert>
                        </div>
                    </div>

                    <button class="btn btn-info mr-1 btn-submit" type="submit"><i class="fa fa-paper-plane"></i>
                     UPDATE
                    </button>
                    <button class="btn btn-warning btn-reset" type="reset"><i class="fa fa-redo"></i>
                     RESET
                    </button>
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
            title: 'Edit Tag - MasKahfi.com - Belajar terus sampai pintar',
        }
    },

    data() {
        return {
            tag: '',
            validation: [],
        }
    },

    mounted() {
        this.$axios.get(`/api/admin/tags/${this.$route.params.id}`)
        .then(response => {
            this.tag = response.data.data.name
        })
    },

    methods: {
        async updateTag() {
            await this.$axios.put(`/api/admin/tags/${this.$route.params.id}`, {
                name: this.tag
            })
            .then(() => {
                this.$swal.fire({
                    title: 'BERHASIL!',
                    text: "Data Berhasil di Update!",
                    icon: 'success',
                    showConfirmButton: false,
                    timer: 2000
                })
                this.$router.push({
                    name: 'admin-tag'
                })
            })
            .catch(error => {
                this.validation = error.response.data
            })
        }
    }
}

</script>

<style>

</style>