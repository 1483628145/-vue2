<template>
  <div class="clearfix selector">
    <div class="type-wrap logo">
      <div class="fl key brand">品牌</div>
      <div class="value logos">
        <ul class="logo-list">
          <!-- 品牌列表 -->

          <li
            v-for="item in trademarkList"
            :key="item.tmId"
            @click="changeTraderMark(item)"
          >
            {{ item.tmName }}
          </li>
        </ul>
      </div>
      <div class="ext">
        <a href="javascript:void(0);" class="sui-btn">多选</a>
        <a href="javascript:void(0);">更多</a>
      </div>
    </div>
    <!-- 商品属性 -->
    <div class="type-wrap" v-for="att in attrsList" :key="att.attrId">
      <!-- 属性名 -->
      <div class="fl key">{{ att.attrName }}</div>
      <div class="fl value">
        <ul class="type-list">
          <!-- 商品属性对应的属性值 -->
          <li
            v-for="(attr, index) in att.attrValueList"
            :key="index"
            @click="changeAttr(att, attr)"
          >
            <a>{{ attr }}</a>
          </li>
        </ul>
      </div>
      <div class="fl ext"></div>
    </div>
  </div>
</template>

<script>
import { mapGetters } from "vuex";
export default {
  name: "SearchSelector",
  // 从vuex中拿到数据
  computed: {
    ...mapGetters("search", ["trademarkList", "attrsList"]),
  },
  methods: {
    // 改变品牌 将数据传递到父组件
    changeTraderMark(trademark) {
      // 将数据传递给父组件
      this.$emit("changeTardeMark", trademark);
      // console.log(trademark);
    },
    // 改变商品属性 将数据传递到search组件
    changeAttr(att, attr) {
      // 将数据处理好 ["属性ID:属性值:属性名"]
      // console.log(att.attrId, att.attrName, attr);
      this.$emit("changeAttr", att, attr);
    },
  },
};
</script>

<style lang="less" scoped>
.selector {
  border: 1px solid #ddd;
  margin-bottom: 5px;
  overflow: hidden;

  .logo {
    border-top: 0;
    margin: 0;
    position: relative;
    overflow: hidden;

    .key {
      padding-bottom: 87px !important;
    }
  }

  .type-wrap {
    margin: 0;
    position: relative;
    border-top: 1px solid #ddd;
    overflow: hidden;

    .key {
      width: 100px;
      background: #f1f1f1;
      line-height: 26px;
      text-align: right;
      padding: 10px 10px 0 15px;
      float: left;
    }

    .value {
      overflow: hidden;
      padding: 10px 0 0 15px;
      color: #333;
      margin-left: 120px;
      padding-right: 90px;

      .logo-list {
        li {
          float: left;
          border: 1px solid #e4e4e4;
          margin: -1px -1px 0 0;
          width: 105px;
          height: 52px;
          text-align: center;
          line-height: 52px;
          overflow: hidden;
          text-overflow: ellipsis;
          white-space: nowrap;
          font-weight: 700;
          color: #e1251b;
          font-style: italic;
          font-size: 14px;

          img {
            max-width: 100%;
            vertical-align: middle;
          }
        }
      }

      .type-list {
        li {
          float: left;
          display: block;
          margin-right: 30px;
          line-height: 26px;

          a {
            text-decoration: none;
            color: #666;
          }
        }
      }
    }

    .ext {
      position: absolute;
      top: 10px;
      right: 10px;

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
        padding: 0 10px;
        background: #fff;
        border: 1px solid #d5d5d5;
      }

      a {
        color: #666;
      }
    }
  }
}
</style>
