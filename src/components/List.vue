<template>
    <div class="row">
      <div class="card" v-for="user in info.users" :key="user.login">
        <a :href="user.html_url" target="_blank">
          <img :src="user.avatar_url" style='width: 100px'/>
        </a>
        <p class="card-text">{{user.login}}</p>
      </div>
      <div v-show="info.isFirst" style="font-size: 48px">Welcome！</div>
      <div v-show="info.isLoading" style="font-size: 48px">Loading...</div>
      <div v-show="info.errMsg">{{info.errMsg}}</div>
    </div>
</template>

<script>
    export default {
        name: 'List',
        data() {
          return {
            info: {
              isFirst: true,
              isLoading: false,
              errMsg: '',
              users: []
            }
          }
        },
        mounted() {
          this.$bus.$on('getUsers', (dataObj) => {
            this.info = {...this.info, ...dataObj}
          })
        },
        beforeDestroy() {
          this.$bus.$off('getUsers')
        },
    }
</script>

<style scoped>
    .album {
        min-height: 50rem; /* Can be removed; just added for demo purposes */
        padding-top: 3rem;
        padding-bottom: 3rem;
        background-color: #f7f7f7;
    }

    .card {
        float: left;
        width: 33.333%;
        padding: .75rem;
        margin-bottom: 2rem;
        border: 1px solid #efefef;
        text-align: center;
    }

    .card > img {
        margin-bottom: .75rem;
        border-radius: 100px;
    }

    .card-text {
        font-size: 85%;
    }

</style>