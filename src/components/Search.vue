<template>
    <section class="jumbotron">
      <h3 class="jumbotron-heading">Search Github Users</h3>
      <div>
        <input type="text" placeholder="enter the name you search" v-model="keyword"/>&nbsp;<button @click="searchUsers">Search</button>
      </div>
    </section>
</template>

<script>
    import axios from 'axios'
    export default {
        name: 'Search',
        data() {
          return {
            keyword: '',
          }
        },
        methods: {
          searchUsers() {
            // 请求前更新List数据
            this.$bus.$emit('getUsers', {isFirst: false, isLoading: true, errMsg: '', users: []}),
            axios.get(`https://api.github.com/search/users?q=${this.keyword}`).then(
              response => {
                // 请求成功更新数据
                console.log('收到用户数据')
                this.$bus.$emit('getUsers', {isLoading: false, errMsg: '', users: response.data.items})
              },
              error => {
                // 请求失败更新数据
                console.log(err.message)
                this.$bus.$emit('getUsers', {isLoading: false, errMsg: error.message, users: []})
              }
            )
          }
        },
        
    }
</script>

<style>

</style>