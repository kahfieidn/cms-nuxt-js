<template>
    <div>
        <b-jumbotron class="p-3 rounded-0 mb-0">
            <b-container>
                <b-row class="mt-2">
                    <b-col md="1" class="text-center">
                        <b-img src="https://i.imgur.com/zkFaIRQ.jpg" rounded="circle" width="70"></b-img>
                    </b-col>
                    <b-col md="11">
                        <h3 class="font-weight-bold">Mas Kahfi</h3>
                        <p>Belajar terus sampai pintar</p>
                    </b-col>
                </b-row>
            </b-container>
        </b-jumbotron>

        <b-navbar toggleable="lg" type="dark" class="bg-navbar">
            <b-container>
                <b-navbar-toggle target="nav-collapse"></b-navbar-toggle>

                <b-collapse id="nav-collapse" is-nav>
                    <b-navbar-nav>
                        <b-nav-item v-for="menu in menus" :key="menu.id" :to="menu.url">{{ menu.name.toUpperCase() }}</b-nav-item>
                    </b-navbar-nav>

                    <b-navbar-nav class="ml-auto">
                        <b-form-input v-model="search" @keypress.enter="searchData" size="sm" class="mr-sm-2 border-0" placeholder="tulis kata kunci...">
                        </b-form-input>
                        <b-button @click="searchData" size="sm" class="my-2 my-sm-0" variant="primary">CARI</b-button>
                    </b-navbar-nav>
                </b-collapse>
            </b-container>
        </b-navbar>
    </div>
</template>

<script>
export default {

    //data function
    data() {
      return {
        //state tags
        menus: [],

        search: ''
      }
    },

    async fetch() {

      //fething tags on Rest API
      await this.$axios.get('/api/web/menus')
      .then(response => {

        //assign response to state "tags"
        this.menus = response.data.data
      })
    },

    methods: {
        searchData() {
            this.$router.push({
                name: 'search',
                query: {
                    q: this.search
                }
            });
        }
    }

}
</script>


<style>

</style>