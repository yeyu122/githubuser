<template>
  <section class="jumbotron">
    <h3 class="jumbotron-heading">搜索 github 用户</h3>
    <div>
      <input
        type="text"
        placeholder="按下搜索按钮"
        v-model="keyWord"
      />&nbsp;
      <button @click="searchUsers">搜索</button>
    </div>
  </section>
</template>

<script>
import axios from "axios";
export default {
  // eslint-disable-next-line vue/multi-word-component-names
  name:'Search',
  data() {
    return {
      keyWord: "",
    }
  },
  methods: {
    searchUsers() {
      //请求前更新List的数据
      this.$bus.$emit("updateListData", {
        isLoading: true,
        errMsg: "",
        users: [],
        isFirst: false,
      });
      axios.get(`https://api.github.com/search/users?q=${this.keyWord}`).then(
        (response) => {
          console.log("请求成功了");
          console.log(response);
          this.$bus.$emit("updateListData", {
            //请求成功后更新List的数据
            isLoading: false,
            errMsg: "",
            users: response.data.items,
          });
        },
        (error) => {
          this.$bus.$emit("updateListData", {
            //请求后更新List的数据
            isLoading: false,
            errMsg: error.message,
            users: [],
          });
        }
      );
    },
  },
};
</script>