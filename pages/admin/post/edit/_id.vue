<template>
    <div class="content-wrapper mb-5">
        <section class="content-header">
            <div class="container-fluid">
            </div>
        </section>

        <section class="content">
            <div class="card card-outline card-info">
                <div class="card-header">
                    <h3 class="card-title"><i class="nav-icon fas fa-book-open"></i> EDIT POST</h3>
                    <div class="card-tools">
                        <button class="btn btn-tool" data-card-widget="collapse" title="Collapse">
                            <i class="fas fa-minus"></i>
                        </button>
                        <button class="btn btn-tool" data-card-widget="remove">
                            <i class="fas fa-times"></i>
                        </button>
                    </div>
                </div>
                <div class="card-body">
                    <form @submit.prevent="updatePost">
                        <div class="form-group">
                            <label>GAMBAR POST</label>
                            <input type="file" @change="handleFileChange" class="form-control">                            
                        </div>
                        <div class="form-group">
                            <label>JUDUL POST</label>
                            <input type="text" v-model="post.title" placeholder="Masukkan Judul Post" class="form-control"> 
                            <div class="mt-2" v-if="validation.title">
                                <b-alert show variant="danger">{{ validation.title[0] }}</b-alert>
                            </div>
                        </div>                           
                        <div class="form-group">
                            <label>CATEGORY</label>
                            <multiselect v-model="post.category_id" :options="categories" label="name" track-by="id" :searchable="true"></multiselect>
                            <div class="mt-2" v-if="validation.category_id">
                                <b-alert show variant="danger">{{ validation.categroy_id[0] }}</b-alert>
                            </div>
                        </div>                           
                        <div class="form-group">
                            <label>KONTENT POST</label>
                            <client-only placeholder="loading...">
                                <ckeditor-nuxt v-model="post.content" :config="editorConfig">
                            </ckeditor-nuxt>
                            </client-only>
                            <div class="mt-2" v-if="validation.content">
                                <b-alert show variant="danger">{{ validation.content[0] }}</b-alert>
                            </div>
                        </div>               
                        
                        <div class="form-group">
                            <label>TAGS</label>
                            <multiselect v-model="post.tags" :options="tags" label="name" track-by="id" :multiple="true" :searchable="true"></multiselect>
                        </div>

                        <div class="form-group">
                            <label>DESCRIPTION</label>
                            <textarea v-model="post.description" class="form-control" rows="3" placeholder="Masukkan Deskripsi Singkat"></textarea>
                            <div class="mt-2" v-if="validation.description">
                            <b-alert show variant="danger">{{ validation.description[0] }}</b-alert>
                            </div>
                        </div>

                        <button class="btn btn-info mr-1 btn-submit" type="submit"><i class="fa fa-paper-plane"></i>
                         UPDATE</button>
                        <button class="btn btn-warning mr-1 btn-reset" type="reset"><i class="fa fa-paper-redo"></i>
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
            title: 'Edit Post - MasKahfi.com - Belajar Sampai Pintar'
        }
    },

    components: {
        'ckeditor-nuxt': () => {
            if(process.client){
                return import('@blowstack/ckeditor-nuxt')
            }
        },
    },

    data() {
        return {
            post: {
                image: '',
                title: '',
                category_id: '',
                content: '',
                description: '',
                tags: []
            },

            categories: [],

            tags: [],

            validation: [],

            editorConfig: {
                removePlugins: ['Title'],
                simpleUpload: {
                    uploadUrl: 'https://api-nuxt.appkepri.web.id/api/web/posts/storeImage'
                }
            }
        }
    },

    mounted() {
        this.$axios.get(`/api/admin/posts/${this.$route.params.id}`)
        .then(response => {
            this.post.title = response.data.data.title
            this.post.category_id = response.data.data.category
            this.post.content = response.data.data.content
            this.post.tags = response.data.data.tags
            this.post.description = response.data.data.description
        })

        this.$axios.get('/api/admin/categories')
        .then(response => {
            this.categories = response.data.data.data
        })

        this.$axios.get('/api/admin/tags')
        .then(response => {
            this.tags = response.data.data.data
        })
    },

    methods: {
        handleFileChange(e) {
            let image = this.post.image = e.target.files[0]

            if(!image.type.match('image.*')){
                e.target.value = ''
                this.post.image = null

                this.$swal.fire({
                    title: 'OOPS!',
                    text: "Format File Tidak Didukung!",
                    icon: 'error',
                    showConfirmButton: false,
                    timer: 2000
                })
            }
        },

        async updatePost(){
            let tags = this.post.tags
            let selectedTags = []

            tags.forEach((tag) => {
                selectedTags.push(tag.id)
            })

            let formData = new FormData();
            formData.append('image', this.post.image)
            formData.append('title', this.post.title)
            formData.append('category_id', this.post.category_id ? this.post.category_id.id : '')
            formData.append('content', this.post.content)
            formData.append('description', this.post.description)
            formData.append("_method", "PATCH")

            for(var i = 0; i < selectedTags.length; i++){
                formData.append('tags[]', selectedTags[i]);
            }
            await this.$axios.post(`/api/admin/posts/${this.$route.params.id}`, formData)
            .then(() => {
                this.$swal.fire({
                    title: 'BERHASIL!',
                    text: "Data Berhasil diUpdate!",
                    icon: 'success',
                    showConfirmButton: false,
                    timer: 2000
                })

                this.$router.push({
                    name: 'admin-post'
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

.ck-editor__editable {
    min-height: 200px;
}

</style>