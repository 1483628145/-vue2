<template>
  <div>
    <TypeNav />
    <div class="main">
      <div class="py-container">
        <!--bread-->
        <div class="bread">
          <ul class="fl sui-breadcrumb">
            <li>
              <a href="#">全部结果</a>
            </li>
          </ul>
          <!-- 搜索结果 -->
          <ul class="fl sui-tag">
            <!-- 分类面包屑 -->
            <li class="with-x" v-if="searchParams.categoryName">
              {{ searchParams.categoryName
              }}<i @click="removeCategoryName">×</i>
            </li>
            <!-- 关键字面包屑 -->
            <li class="with-x" v-if="searchParams.keyword">
              {{ searchParams.keyword }}<i @click="removeKeyword">×</i>
            </li>
            <!-- 品牌面包屑 -->
            <li class="with-x" v-if="searchParams.trademark">
              {{ searchParams.trademark.split(":")[1]
              }}<i @click="removeTardeMark">×</i>
            </li>
            <!-- 商品属性面包屑 -->
            <li
              class="with-x"
              v-for="(item, index) in searchParams.props"
              :key="index"
            >
              {{ item.split(":")[1] }}
              <i @click="removeAttr(index)">×</i>
            </li>
          </ul>
        </div>

        <!--selector-->
        <SearchSelector
          @changeTardeMark="tarderMarket"
          @changeAttr="changeAttr"
        />

        <!--details-->
        <div class="details clearfix">
          <div class="sui-navbar">
            <div class="navbar-inner filter">
              <ul class="sui-nav">
                <!-- 搜索排序1: 综合,2: 价格 asc: 升序,desc: 降序 示例: "1:desc" -->
                <!-- 点击后出现类名 -->
                <li :class="{ active: isOne }">
                  <a href="#">综合<span v-if="isOne"></span></a>
                </li>
                <li :class="{ active: isTwo }">
                  <a href="#"
                    >价格
                    <span
                      v-if="isTwo"
                      class="iconfont icon-jiantoushang"
                    ></span
                  ></a>
                </li>
                <li>
                  <a href="#"
                    >价格<span class="icon-jiantou-xia iconfont"></span
                  ></a>
                </li>
              </ul>
            </div>
          </div>
          <!-- 商品列表 -->
          <div class="goods-list">
            <ul class="yui3-g">
              <li class="yui3-u-1-5" v-for="item in goodsList" :key="item.id">
                <div class="list-wrap">
                  <div class="p-img">
                    <a href="item.html" target="_blank"
                      ><img :src="item.defaultImg"
                    /></a>
                  </div>
                  <div class="price">
                    <strong>
                      <em>¥</em>
                      <i>{{ item.price }}</i>
                    </strong>
                  </div>
                  <div class="attr">
                    <a
                      target="_blank"
                      href="item.html"
                      title="促销信息，下单即赠送三个月CIBN视频会员卡！【小米电视新品4A 58 火爆预约中】"
                      >{{ item.title }}</a
                    >
                  </div>
                  <div class="commit">
                    <i class="command">已有<span>2000</span>人评价</i>
                  </div>
                  <div class="operate">
                    <a
                      href="success-cart.html"
                      target="_blank"
                      class="sui-btn btn-bordered btn-danger"
                      >加入购物车</a
                    >
                    <a href="javascript:void(0);" class="sui-btn btn-bordered"
                      >收藏</a
                    >
                  </div>
                </div>
              </li>
            </ul>
          </div>
          <!-- 分页 -->
          <div class="fr page">
            <div class="sui-pagination clearfix">
              <ul>
                <li class="prev disabled">
                  <a href="#">«上一页</a>
                </li>
                <li class="active">
                  <a href="#">1</a>
                </li>
                <li>
                  <a href="#">2</a>
                </li>
                <li>
                  <a href="#">3</a>
                </li>
                <li>
                  <a href="#">4</a>
                </li>
                <li>
                  <a href="#">5</a>
                </li>
                <li class="dotted"><span>...</span></li>
                <li class="next">
                  <a href="#">下一页»</a>
                </li>
              </ul>
              <div><span>共10页&nbsp;</span></div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import SearchSelector from "./SearchSelector/SearchSelector";
import { mapGetters } from "vuex";
export default {
  name: "Search",
  components: {
    SearchSelector,
  },
  data() {
    return {
      searchParams: {
        category1Id: "",
        category2Id: "",
        category3Id: "",
        categoryName: "",
        keyword: "",
        // 排序asc:升序 desc:降序
        order: "2:desc",
        pageNo: 1,
        pageSize: 10,
        // 平台售卖参数
        props: [],
        trademark: "",
      },
    };
  },
  computed: {
    // 拿vuex数据
    ...mapGetters("search", ["goodsList", "attrsList", "trademarkList"]),
    // 判断排序是否存在背景色
    isOne() {
      return this.searchParams.order.indexOf("1") != -1;
    },
    isTwo() {
      return this.searchParams.order.indexOf("2") != -1;
    },
  },
  watch: {
    // 监听路由变化
    // 根据路由的变化重复发请求
    $route(newvlaue, oldvalue) {
      // 重新整理请求
      // 将后面俩个对象赋值给前面这个对象
      this.clearParams();

      Object.assign(this.searchParams, this.$route.query, this.$route.params);
      // 发请求拿到新的json
      this.getProductList();
      // this.clearParams();
    },
  },
  beforeMount() {
    // 整理query和params参数 然后在将这些数据传递到getProduceList这个方法里面
    // 显然在这个组件是可以拿到路由的query和params参数的
    // console.log(this.$route.params);

    // 处理参数 更新请求参数  更好的方法是使用 Object.assin
    // this.searchParams.category1Id = this.$route.query.category1Id;
    // this.searchParams.category2Id = this.$route.query.category2Id;
    // this.searchParams.category3Id = this.$route.query.category3Id;
    // this.searchParams.categoryName = this.$route.query.categoryName;
    // this.searchParams.keyword = this.$route.params.keyword;

    // 使用 Object.assign 合并对象
    // 相关语法：后面的对象的参数 添加到第一个对象里面
    Object.assign(this.searchParams, this.$route.query, this.$route.params);

    // 在处理完后再调用接口就可以通过路由参数拿到数据
    // console.log(this.searchParams.category1Id);
  },
  mounted() {
    this.getProductList();
  },
  methods: {
    // 获取搜索结果 -- 面包屑展示数据
    getProductList() {
      this.$store.dispatch("search/getProduceList", this.searchParams);
    },
    // 置空请求id
    clearParams() {
      this.searchParams.category1Id = undefined;
      this.searchParams.category2Id = undefined;
      this.searchParams.category3Id = undefined;
    },
    // 删除分类名字
    removeCategoryName() {
      // 先置空分类名字
      this.searchParams.categoryName = undefined;
      // 将id也置空
      this.clearParams();
      // this.$route.push()
      // 重新发请求
      this.getProductList();
      // 路由跳转 带params的时候要带上 不带的时候直接跳转到自己页面
      if (this.$route.params) {
        this.$router.push({
          name: "search",
          params: this.$route.params,
        });
      } else {
        this.$router.push({ name: "search" });
      }
    },
    // 删除关键字
    removeKeyword() {
      // 删除路由的params参数
      // this.$route.params = {};
      // 删除keyword
      this.searchParams.keyword = undefined;
      // 不需要清空id
      // 重新发请求
      this.getProductList();
      // 将搜索框里面的值置空 通知header兄弟组件删除keyword
      // 通过$bus 提示header组件删除keyword
      this.$bus.$emit("clearKeyword");
      // 页面发生跳转
      if (this.$route.query) {
        this.$router.push({
          name: "search",
          query: this.$route.query,
          params: {},
        });
      } else {
        this.$router.push({ name: "search", params: {} });
      }
    },
    // 删除品牌
    removeTardeMark() {
      this.searchParams.trademark = undefined;
      this.getProductList();
    },
    // 删除商品属性
    removeAttr(index) {
      // 按照索引指定删除数组的某一个值
      if (index > -1 && index < this.searchParams.props.length) {
        this.searchParams.props.splice(index, 1);
      }
      this.getProductList();
    },
    // 接收子组件传递过来的品牌数据
    tarderMarket(trademark) {
      // console.log(trademark);
      // 拿到数据 整理数据 将数据整理成： ID：name的形式
      this.searchParams.trademark = `${trademark.tmId}:${trademark.tmName}`;
      this.getProductList();
    },
    // 接收子组件传递过来的商品参数： ["属性ID:属性值:属性名"]
    changeAttr(att, attr) {
      // 整理数据
      const prop = `${att.attrId}:${attr}:${att.attrName}`;
      // 数组去重
      if (this.searchParams.props.indexOf(prop) === -1) {
        this.searchParams.props.push(prop);
      }
      // 发请求
      this.getProductList();
      // console.log(att.attrId, att.attrName, attr);
    },
  },
};
</script>

<style lang="less" scoped>
.main {
  margin: 10px 0;

  .py-container {
    width: 1200px;
    margin: 0 auto;

    .bread {
      margin-bottom: 5px;
      overflow: hidden;

      .sui-breadcrumb {
        padding: 3px 15px;
        margin: 0;
        font-weight: 400;
        border-radius: 3px;
        float: left;

        li {
          display: inline-block;
          line-height: 18px;

          a {
            color: #666;
            text-decoration: none;

            &:hover {
              color: #4cb9fc;
            }
          }
        }
      }

      .sui-tag {
        margin-top: -5px;
        list-style: none;
        font-size: 0;
        line-height: 0;
        padding: 5px 0 0;
        margin-bottom: 18px;
        float: left;

        .with-x {
          font-size: 12px;
          margin: 0 5px 5px 0;
          display: inline-block;
          overflow: hidden;
          color: #000;
          background: #f7f7f7;
          padding: 0 7px;
          height: 20px;
          line-height: 20px;
          border: 1px solid #dedede;
          white-space: nowrap;
          transition: color 400ms;
          cursor: pointer;

          i {
            margin-left: 10px;
            cursor: pointer;
            font: 400 14px tahoma;
            display: inline-block;
            height: 100%;
            vertical-align: middle;
          }

          &:hover {
            color: #28a3ef;
          }
        }
      }
    }

    .details {
      margin-bottom: 5px;

      .sui-navbar {
        overflow: visible;
        margin-bottom: 0;

        .filter {
          min-height: 40px;
          padding-right: 20px;
          background: #fbfbfb;
          border: 1px solid #e2e2e2;
          padding-left: 0;
          border-radius: 0;
          box-shadow: 0 1px 4px rgba(0, 0, 0, 0.065);

          .sui-nav {
            position: relative;
            left: 0;
            display: block;
            float: left;
            margin: 0 10px 0 0;

            li {
              float: left;
              line-height: 18px;

              a {
                display: block;
                cursor: pointer;
                padding: 11px 15px;
                color: #777;
                text-decoration: none;
              }

              &.active {
                a {
                  background: #e1251b;
                  color: #fff;
                }
              }
            }
          }
        }
      }

      .goods-list {
        margin: 20px 0;

        ul {
          display: flex;
          flex-wrap: wrap;

          li {
            height: 100%;
            width: 20%;
            margin-top: 10px;
            line-height: 28px;

            .list-wrap {
              .p-img {
                padding-left: 15px;
                width: 215px;
                height: 255px;

                a {
                  color: #666;

                  img {
                    max-width: 100%;
                    height: auto;
                    vertical-align: middle;
                  }
                }
              }

              .price {
                padding-left: 15px;
                font-size: 18px;
                color: #c81623;

                strong {
                  font-weight: 700;

                  i {
                    margin-left: -5px;
                  }
                }
              }

              .attr {
                padding-left: 15px;
                width: 85%;
                overflow: hidden;
                margin-bottom: 8px;
                min-height: 38px;
                cursor: pointer;
                line-height: 1.8;
                display: -webkit-box;
                -webkit-box-orient: vertical;
                -webkit-line-clamp: 2;

                a {
                  color: #333;
                  text-decoration: none;
                }
              }

              .commit {
                padding-left: 15px;
                height: 22px;
                font-size: 13px;
                color: #a7a7a7;

                span {
                  font-weight: 700;
                  color: #646fb0;
                }
              }

              .operate {
                padding: 12px 15px;

                .sui-btn {
                  display: inline-block;
                  padding: 2px 14px;
                  box-sizing: border-box;
                  margin-bottom: 0;
                  font-size: 12px;
                  line-height: 18px;
                  text-align: center;
                  vertical-align: middle;
                  cursor: pointer;
                  border-radius: 0;
                  background-color: transparent;
                  margin-right: 15px;
                }

                .btn-bordered {
                  min-width: 85px;
                  background-color: transparent;
                  border: 1px solid #8c8c8c;
                  color: #8c8c8c;

                  &:hover {
                    border: 1px solid #666;
                    color: #fff !important;
                    background-color: #666;
                    text-decoration: none;
                  }
                }

                .btn-danger {
                  border: 1px solid #e1251b;
                  color: #e1251b;

                  &:hover {
                    border: 1px solid #e1251b;
                    background-color: #e1251b;
                    color: white !important;
                    text-decoration: none;
                  }
                }
              }
            }
          }
        }
      }

      .page {
        width: 733px;
        height: 66px;
        overflow: hidden;
        float: right;

        .sui-pagination {
          margin: 18px 0;

          ul {
            margin-left: 0;
            margin-bottom: 0;
            vertical-align: middle;
            width: 490px;
            float: left;

            li {
              line-height: 18px;
              display: inline-block;

              a {
                position: relative;
                float: left;
                line-height: 18px;
                text-decoration: none;
                background-color: #fff;
                border: 1px solid #e0e9ee;
                margin-left: -1px;
                font-size: 14px;
                padding: 9px 18px;
                color: #333;
              }

              &.active {
                a {
                  background-color: #fff;
                  color: #e1251b;
                  border-color: #fff;
                  cursor: default;
                }
              }

              &.prev {
                a {
                  background-color: #fafafa;
                }
              }

              &.disabled {
                a {
                  color: #999;
                  cursor: default;
                }
              }

              &.dotted {
                span {
                  margin-left: -1px;
                  position: relative;
                  float: left;
                  line-height: 18px;
                  text-decoration: none;
                  background-color: #fff;
                  font-size: 14px;
                  border: 0;
                  padding: 9px 18px;
                  color: #333;
                }
              }

              &.next {
                a {
                  background-color: #fafafa;
                }
              }
            }
          }

          div {
            color: #333;
            font-size: 14px;
            float: right;
            width: 241px;
          }
        }
      }
    }
  }
}
</style>
