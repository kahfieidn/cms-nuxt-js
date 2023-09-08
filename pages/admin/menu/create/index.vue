<template>
    <div class="content-wrapper">
        <section class="content-header">
            <div class="container-fluid">

            </div>
        </section>

        <section class="content">
            <div class="card card-outline card-info">
                <div class="card-header">
                    <h3 class="card-title"><i class="nav-icon fas fa-clone"></i> TAMBAH</h3>
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
                    <form @submit.prevent="storeMenu">
                        <div class="form-group">
                            <label>NAMA MENU</label>
                            <input type="text" class="form-control" v-model="menu.name" placeholder="Masukkan Nama Menu">
                            <div class="mt-2" v-if="validation.name">
                            <b-alert show variant="danger">{{ validation.name[0] }}</b-alert>
                            </div>
                        </div>
                        <div class="form-group">
                            <label>URL MENU</label>
                            <input type="text" class="form-control" v-model="menu.url" placeholder="Masukkan Url Menu">
                            <div class="mt-2" v-if="validation.url">
                            <b-alert show variant="danger">{{ validation.url[0] }}</b-alert>
                            </div>
                        </div>

                        <button class="btn btn-info mr-1 btn-submit" type="submit"><i class="fa fa-paper-plane"></i>
                         SIMPAN</button>
                        <button class="btn btn-warning btn-reset" type="reset"><i class="fa fa-redo"></i>
                         RESET</button>
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
        return{
            title: 'Tambah Menu - MasKahfi.com - Belajar Sampai pintar'
        }
    },

    data() {
        return {
            menu: {
                name: '',
                url: '',
            },
            validation: []
        }
    },

    methods: {
        async storeMenu() {
            await this.$axios.post(`/api/admin/menus`, {
                name: this.menu.name,
                url: this.menu.url,
            })
            .then(() => {
                this.$swal.fire({
                    title: 'BERHASIL',
                    text: "Data Berhasil Disimpan",
                    icon: 'success',
                    showConfirmButton: false,
                    timer: 2000
                })
                this.$router.push({
                    name: 'admin-menu'
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