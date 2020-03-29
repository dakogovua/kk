<template>
    <div  v-if="show" class="search">
        <!--Search  {{ searchProp }}-->
        <h3>Search results:</h3>
        <p  v-for="data,key in info.data.results">
            <router-link :to="{name: 'details', params: {link: data.url}}">
                <!--{{ data.name   }} {{ data.title }}-->
                {{ info.config.url.includes('films') ? data.title : data.name }}

            </router-link>
        </p>

    </div>
</template>

<script>

    export default {
        props: {
            searchProp: String,
        },
        data(){
            return{
                show: false,
                info: ''
            }
        },
        watch: {
            $route: "watch"
        },
        methods:{
            watch(){
                this.show = false
            },
            action(data){
                let self = this
                let urlquery = this.$route.params.link+'?search='+self.searchProp
                console.log ('urlquery', urlquery)
                this.$axios
                    .get(urlquery)
                    .then(response => {
                        //   console.log('then', response)
                        this.info = response
                        this.show = true
                    })
                    .catch(error => {
                        this.loading = false
                        console.log(error)
                    });
            }
        },


    }
</script>

<style scoped>
    .search a {
        color: #0b58a2;
    }
</style>