<!--
  - storeAdd.vue Vue 文件
  - @JsName storeAdd.vue
  - @Description
  - @DateTime 2017/12/6 15:41
  - @author 亚茹
  -->
<template>
  <div>
    <m-Breadcrumb mTitle1="店铺管理" mTitle2="店铺添加"></m-Breadcrumb>
    <div class="storeAdd_content">
      <div class="storeAddContent">
        <!--店铺名称-->
        <div class="branchName"><label>店铺名称</label><input type="text"></div>
        <!--店铺编码-->
        <div class="branchCode"><label>店铺编码</label><input type="number"></div>
        <!--店铺状态-->
        <div class="status">
          <label>店铺状态</label>
          <el-radio-group v-model="radio1">
            <el-radio :label="1">营业中</el-radio>
            <el-radio :label="2">休息中</el-radio>
            <el-radio :label="3">已注销</el-radio>
          </el-radio-group>
        </div>
        <!--店铺属性-->
        <div class="property">
          <label>店铺属性</label>
          <el-radio-group v-model="radio2">
            <el-radio :label="4">本地门店</el-radio>
            <el-radio :label="5">外地门店</el-radio>
          </el-radio-group>
        </div>
        <!--店铺分组-->
        <div class="group">
          <label>店铺分组</label>
          <el-select v-model="value" placeholder="请选择">
            <el-option
              v-for="item in options"
              :key="item.value"
              :label="item.label"
              :value="item.value">
            </el-option>
          </el-select>
        </div>
        <!--店铺面积-->
        <div class="acreage"><label>店铺面积</label><input type="text"></div>
        <!--店铺类型-->
        <div class="branchType">
          <label>店铺类型</label>
          <el-radio-group v-model="radio3">
            <el-radio :label="6">自营店</el-radio>
            <el-radio :label="7">加盟店</el-radio>
          </el-radio-group>
        </div>
        <!--商圈-->
        <div class="district">
          <label class="labelStyle">商 圈</label>
          <el-radio-group v-model="radio4">
            <el-radio :label="8">默认</el-radio>
            <el-radio :label="9">生活社区</el-radio>
            <el-radio :label="10">商业中心</el-radio>
            <el-radio :label="11">休闲娱乐</el-radio>
            <el-radio :label="12">学区</el-radio>
            <el-radio :label="13">交通枢纽</el-radio>
          </el-radio-group>
        </div>
        <!--营业时间-->
        <div class="openingTime"><label>营业时间</label><input type="text"></div>
        <!--管理模式-->
        <div class="manageMode">
          <label>管理模式</label>
          <el-radio-group v-model="radio5">
            <el-radio :label="14">紧密型</el-radio>
          </el-radio-group>
        </div>
        <!--起送价格-->
        <div class="reservePrice"><label>起送价格</label><input type="number"></div>
        <!--排序权重-->
        <div class="sortOrder"><label>排序权重</label><input type="number"></div>
        <!--店铺描述-->
        <div class="branchDes"><label>店铺描述</label><textarea id="" cols="30" rows="3"></textarea></div>

      </div>
      <div class="storeAdd_button">
        <input type="button" value="保存">
      </div>
    </div>
  </div>
</template>

<script>
  import mBreadcrumb from './../../component/headerTag.vue'
  export default {
    name: 'storeAdd',
    data () {
      return {
        radio1: 1, // 店铺状态
        radio2: 4, // 店铺属性
        radio3: 6, // 店铺类型
        radio4: 8, // 商圈
        radio5: 14, // 管理模式
        options: [{
          value: '选项1',
          label: '黄金糕'
        }, {
          value: '选项2',
          label: '双皮奶'
        }, {
          value: '选项3',
          label: '蚵仔煎'
        }],
        value: ''
      }
    },
    methods: {
    },
    mounted () {
    },
    components: {
      mBreadcrumb
    }
  }
</script>
<style>
  .group .el-select{
    height: 32px;
  }
  .group .el-select .el-input{
    height: 32px;
  }
  .group .el-select .el-input .el-input__inner{
    height: 32px;
  }
</style>
<!--引入样式-->
<style lang="scss" scoped>@import "scss/storeAdd";</style>
