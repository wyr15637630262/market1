<!--
  - commodityVariety Vue 文件
  - @JsName commodityVariety
  - @Description
  - @DateTime 2017/12/11 16:31
  - @author 亚茹
  -->
<template>
  <div>
    <m-Breadcrumb mTitle1="商品管理" mTitle2="商品类别"></m-Breadcrumb>
    <div class="commodityType_content">
      <div class="query_criteria">
        <div class="query_name"><label>名称</label><input type="text"></div>
        <div class="query_ensure"><input type="button" value="查询"></div>
        <div class="query_recycled" @click="rubish()"><a>回收站</a><img src="./../../../static/images/select.png" v-show="imgShow"></div>
        <div class="query_newAdd" @click="addGoodsTag()"><a>+新增</a></div>
      </div>
      <table class="commodityType_table">
        <thead>
        <tr>
          <td style="width: 16%">编号</td>
          <td style="width: 16%">名称</td>
          <td style="width: 16%">排序</td>
          <td style="width: 16%">商品数</td>
          <td style="width: 20%">创建时间</td>
          <td style="width: 16%">操作</td>
        </tr>
        </thead>
        <tbody v-if="typeList.length!==0">
        <tr v-for="(item,index) in typeList">
          <td>00{{index+1}}</td>
          <td>{{item.labelName}}</td>
          <td>{{item.sortPriority}}</td>
          <td>{{item.productNumber}}</td>
          <td>{{item.createTime|date('YYYY-MM-DD')}}</td>
          <td>
            <a @click="goEditor(item.labelName,item.sortPriority,item.id)">{{msg1}}</a>
            <a style="padding: 0 10px">|</a>
            <a @click="goDelete(item.id)">{{msg2}}</a>
          </td>
        </tr>
        </tbody>
        <tbody v-else-if="typeList.length===0">
        <tr>
          <td colspan="7" >暂无数据...</td>
        </tr>
        </tbody>
      </table>
      <div class="commodityType_page" v-if="typeList.length!==0">
        <div class="pages">
          <el-pagination
            :page-size="typeListPageSize"
            layout="prev, pager, next,jumper"
            :total="typeListTotal"
            @current-change="handleCurrentChange">
          </el-pagination>
        </div>
      </div>
    </div>
    <!--编辑-->
    <div class="mask" v-if="edit">
      <div class="mask_content">
        <div class="maskC_top">
          <a>商品类别</a>
          <a>-</a>
          <a style="color:#ec6a47">编辑类别</a>
        </div>
        <div class="maskC_center">
          <div class="mc_content">
            <div class="tagName">
              <label >类别名称</label>
              <input type="text" v-model="labelName" ref="reviseTagName">
            </div>
            <div class="sort">
              <label >排&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;序</label>
              <input type="number" v-model="sortPriority" ref="reviseTagSort">
            </div>
          </div>
        </div>
        <div class="maskC_bottom">
          <div class="mb_content">
            <input type="button" value="取消" @click="editCancle()">
            <input type="button" value="保存" @click="editConfirm()">
          </div>
        </div>
      </div>
    </div>
    <!--增加-->
    <div class="addMask" v-if="adds">
      <div class="addMask_content">
        <div class="addMaskC_top">
          <a>商品类别</a>
          <a>-</a>
          <a style="color:#ec6a47">新增类别</a>
        </div>
        <div class="addMaskC_center">
          <div class="adc_content">
            <div class="tagName">
              <label >类别名称</label>
              <input type="text" ref="addTagName">
            </div>
            <div class="sort">
              <label >排&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;序</label>
              <input type="number" ref="addTagSort">
            </div>
          </div>
        </div>
        <div class="addMaskC_bottom">
          <div class="adb_content">
            <input type="button" value="取消" @click="addCancle()">
            <input type="button" value="保存" @click="addConfirm()">
          </div>
        </div>
      </div>
    </div>

  </div>
</template>

<script>
  import $ from 'jquery'
  import rq from './../../http/require.js'
  import mBreadcrumb from './../../component/headerTag.vue'
  export default {
    name: 'commodityType',
    data () {
      return {
        typeParams: {isRecycled: 1, page: 1, size: 10}, // 商品类别参数
        recycleTypeParams: {isRecycled: 0, page: 1, size: 10}, // 回收站商品类别参数
        typeList: [], // 商品类别列表
        typeListTotal: 10, // 商品类别总个数
        typeListPageSize: 10, // 每页个数
        pageNumber: '',
        imgShow: false, // 回收站标识
        adds: false, // 新增标识
        edit: false, // 编辑标识
        labelName: '', // 编辑(标签名)
        sortPriority: '', // 编辑(标签排序)
        msg1: '编辑',
        msg2: '删除'
      }
    },
    methods: {
      // 改变页码
      handleCurrentChange: function (val) {
        var that = this
        console.log(val)
        that.typeParams.page = val
        rq.searchAllTag(that.typeParams).then(function (data) {
          if (data.result === '成功') {
            console.log(data)
            that.typeList = data.data.data
          } else if (data.result === '失败') {
            that.typeList = []
          }
        })
      },
      // 编辑或恢复
      goEditor: function (labelName, sortPriority, id) {
        var that = this
        if (that.imgShow === false) {
          that.edit = true
          that.labelName = labelName
          that.sortPriority = sortPriority
        } else {
          rq.backTag(id).then(function (data) {
            if (data.result === '成功') {
              rq.searchAllTag(that.recycleTypeParams).then(function (data) {
                if (data.result === '成功') {
                  that.typeList = data.data.data
                  that.typeListTotal = data.data.totalRecord
                  that.typeListPageSize = data.data.pageSize
                } else if (data.result === '失败') {
                  that.typeList = []
                }
              })
            } else if (data.result === '失败') {
              that.typeList = []
            }
          })
        }
      },
      // 编辑取消
      editCancle: function () {
        this.edit = false
      },
      // 编辑确定
      editConfirm: function () {
        this.edit = false
      },
      // 新增
      addGoodsTag: function () {
        this.adds = true
      },
      // 新增取消
      addCancle: function () {
        this.adds = false
      },
      // 新增确定及验证该标签是否存在
      addConfirm: function () {
        this.adds = false
      },
      // 删除
      goDelete: function (id) {
        console.log(id)
      },
      // 回收站
      rubish: function () {
        var that = this
        that.imgShow = !that.imgShow
        if (that.imgShow === true) {
          that.msg1 = '恢复'
          that.msg2 = '彻底删除'
          rq.searchAllTag(that.recycleTypeParams).then(function (data) {
            if (data.result === '成功') {
              console.log(data)
              that.typeList = data.data.data
              that.typeListTotal = data.data.totalRecord
              that.typeListPageSize = data.data.pageSize
            } else if (data.result === '失败') {
              that.typeList = []
            }
          })
        } else {
          that.msg1 = '编辑'
          that.msg2 = '删除'
          rq.searchAllTag(that.typeParams).then(function (data) {
            if (data.result === '成功') {
              console.log(data)
              that.typeList = data.data.data
              that.typeListTotal = data.data.totalRecord
              that.typeListPageSize = data.data.pageSize
            } else if (data.result === '失败') {
              that.typeList = []
            }
          })
        }
      }
    },
    mounted () {
      var that = this
      rq.searchAllTag(that.typeParams).then(function (data) {
        if (data.result === '成功') {
          console.log(data)
          that.typeList = data.data.data
          that.typeListTotal = data.data.totalRecord
          that.typeListPageSize = data.data.pageSize
        } else if (data.result === '失败') {
          that.typeList = []
        }
      })
    },
    components: {
      mBreadcrumb
    }
  }
</script>
<style>
  .pages .el-pagination .el-pager li.number{
    margin-right: 5px;
    border:1px solid #9c9b9c;
  }
  .pages .el-pagination .el-pager li.number:hover{
    color: #ec6a47;
  }
  .pages .el-pagination button:hover{
    color: #ec6a47;
  }
  .pages .el-pagination .el-pager li.active{
    color: #494d56;
    border:1px solid #494d56;
  }
</style>
<!-- 引入 commodityVariety Scss 文件  -->
<style lang="scss" scoped>@import "scss/commodityType";</style>
