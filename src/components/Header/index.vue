<template>
  <div class="outer">
    <!-- 头部 -->
    <header class="header">
      <!-- 头部的第一行 -->
      <div class="top">
        <div class="container">
          <div class="loginList">
            <p>尚品汇欢迎您！</p>
            <p>
              <span>请</span>
              <router-link to="/login">登录</router-link>
              <router-link class="register" to="/register"
                >免费注册</router-link
              >
            </p>
          </div>
          <div class="typeList">
            <a href="###">我的订单</a>
            <a href="###">我的购物车</a>
            <a href="###">我的尚品汇</a>
            <a href="###">尚品汇会员</a>
            <a href="###">企业采购</a>
            <a href="###">关注尚品汇</a>
            <a href="###">合作招商</a>
            <a href="###">商家后台</a>
          </div>
        </div>
      </div>
      <!--头部第二行 搜索区域-->
      <div class="bottom">
        <h1 class="logoArea">
          <router-link class="logo" title="尚品汇" to="/home">
            <img src="./images/logo.png" alt="" />
          </router-link>
        </h1>
        <div class="searchArea">
          <form action="###" class="searchForm">
            <input
              type="text"
              id="autocomplete"
              class="input-error input-xxlarge"
              v-model="keyword"
            />
            <button
              @click="toSearch"
              class="sui-btn btn-xlarge btn-danger"
              type="button"
            >
              搜索
            </button>
          </form>
        </div>
      </div>
    </header>
  </div>
</template>

<script>
export default {
  data() {
    return {
      keyword: "",
    };
  },
  methods: {
    toSearch() {
      // 路由传参
      // this.$router.push({
      //   name: "search",
      // });
      // 路由传参 传递 props参数
      // this.$router.push(
      //   {
      //     name: "search",
      //     params: {
      //       keyword: this.keyword,
      //     },
      //   }
      //   // 这俩行的意思是 给promise俩个回调 成功和失败 这样就不会执行内部的失败回调
      //   // () => {},
      //   // () => {}
      // );

      // 判断是否存在query参数 如果存在那么拼接参数
      // location：
      const location = {
        name: "search",
        params: { keyword: this.keyword },
      };
      // 判断query参数
      if (this.$route.query) {
        location.query = this.$route.query;
        // push：跳转
        this.$router.push(location);
      }
    },
  },
  mounted() {
    // 监听$bus 清除掉keyword
    this.$bus.$on("clearKeyword", () => {
      this.keyword = "";
    });
  },
};
</script>

<style lang="less" scoped>
.outer {
  .header {
    & > .top {
      background-color: #eaeaea;
      height: 30px;
      line-height: 30px;

      .container {
        width: 1200px;
        margin: 0 auto;
        overflow: hidden;

        .loginList {
          float: left;

          p {
            float: left;
            margin-right: 10px;

            .register {
              border-left: 1px solid #b3aeae;
              padding: 0 5px;
              margin-left: 5px;
            }
          }
        }

        .typeList {
          float: right;

          a {
            padding: 0 10px;

            & + a {
              border-left: 1px solid #b3aeae;
            }
          }
        }
      }
    }

    & > .bottom {
      width: 1200px;
      margin: 0 auto;
      overflow: hidden;

      .logoArea {
        float: left;

        .logo {
          img {
            width: 175px;
            margin: 25px 45px;
          }
        }
      }

      .searchArea {
        float: right;
        margin-top: 35px;

        .searchForm {
          overflow: hidden;

          input {
            box-sizing: border-box;
            width: 490px;
            height: 32px;
            padding: 0px 4px;
            border: 2px solid #ea4a36;
            float: left;

            &:focus {
              outline: none;
            }
          }

          button {
            height: 32px;
            width: 68px;
            background-color: #ea4a36;
            border: none;
            color: #fff;
            float: left;
            cursor: pointer;

            &:focus {
              outline: none;
            }
          }
        }
      }
    }
  }
}
</style>
