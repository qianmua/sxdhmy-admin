<template>
    <div>
      <el-form :inline="true" :model="formInline" class="demo-form-inline">
      <el-form-item label="生产厂家">
        <el-input v-model="formInline.user" placeholder="生产厂家"></el-input>
 
      </el-form-item>
      <el-form-item label="负责人">
        <el-input v-model="formInline.user" placeholder="负责人"></el-input>
      </el-form-item>
      
      <el-form-item>
        <el-button type="primary" @click="onSubmit">查询</el-button>
        <el-button type="primary" @click="queryBatch">精确查询</el-button>
        <el-button type="primary" @click="addOrUpdate">添加厂商</el-button>
        <el-button type="primary" @click="deleteAll">批量删除</el-button>
        
      </el-form-item>
    </el-form>
    <el-table
    :data="listinfo"
    border
    @selection-change="selectChangeEvent"
    style="width: 100%">
    <el-table-column
      type="selection"
      width="55">
    </el-table-column>
    <el-table-column
      fixed
      type="index"
      label="序号"
      width="100">
    </el-table-column>
    <el-table-column
      prop="customName"
      label="客户名称"
      width="120">
    </el-table-column>
    <el-table-column
      prop="contractNo"
      label="合同号"
      width="120">
    </el-table-column>
    <el-table-column
      label="货物数/附件数"
      width="120">
      <template slot-scope="scope">
        {{scope.row.cnumber}}/{{scope.row.extCnumber}}
      </template>
    </el-table-column>
    <el-table-column
      prop="inputBy"
      label="制单人"
      width="80">
    </el-table-column>
    <el-table-column
      prop="checkBy"
      label="审核人"
      width="120">
    </el-table-column>
    <el-table-column
      prop="inspector"
      label="验货员"
      width="100">
    </el-table-column>
    <el-table-column
      prop="signingDate"
      label="签单日期"
      width="200">
    </el-table-column>
    <el-table-column
      prop="deliveryPeriod"
      label="交货期限"
      width="200">
    </el-table-column>
    <el-table-column
      prop="shipTime"
      label="船期"
      width="200">
    </el-table-column>
    <el-table-column
      prop="amount"
      label="总金额"
      width="120">
    </el-table-column>
    
    <el-table-column
      prop="state"
      label="状态"
      width="250">
      <template slot-scope="scope" >
        <el-tag v-show="scope.row.state == '0'">未走货</el-tag>
        <el-tag type="info" v-show="scope.row.state != '1'">草稿</el-tag>
        <el-tag type="success" v-show="scope.row.state != '2'">已上报</el-tag>
      </template>
    </el-table-column>
    <el-table-column
      fixed="right"
      label="操作"
      width="150">
      <template slot-scope="scope">
        
        <el-button @click="nothings" type="text" size="small">货物</el-button>
        <el-button type="text" size="small" @click="updateInfo">修改</el-button>
        <el-button type="text" size="small" @click="deleteInfo(scope.row.contractId)">删除</el-button>
      </template>
    </el-table-column>
  </el-table>
  <el-pagination
      @size-change="handleSizeChange"
      @current-change="handleCurrentChange"
      :current-page.sync="currentPage2"
      :page-sizes="[10, 30, 50, 100]"
      :page-size="10"
      style="text-align:center"
      background
      layout="sizes, prev, pager, next"
      :total="100">
    </el-pagination>
    </div>
</template>
<script>
export default {
  data(){
        return {
           // 条件查询
            formInline: {
              user: '',
              region: ''
            },
            currentPage2: 1,
            current: 1 ,
            limit: 10 ,
            total: 0,
            listinfo: [],
            factoryInfo: {
              factoryId: '',
              state : ''
            }
        }
    },
    created(){
        this.queryProductLIst()
    },
    methods:{
      selectChangeEvent(){

      },
      handleClose(done) {
        this.$confirm('确认关闭？')
          .then(_ => {
            done();
          })
          .catch(_ => {});
      },
      addOrUpdate(){

      },
      deleteAll(){

      },
      queryBatch(){
        
      },
      nothings(){

      },
      updateInfo(){

      },
      deleteInfo(id){
        console.log(id);
        
        //deleteById
          this.$confirm(`此操作将永久删除记录 id:[${id}], 是否继续?`, '提示', {
          confirmButtonText: '确定',
          cancelButtonText: '取消',
          type: 'warning'
          }).then(() => {
            this.$http({
            url: this.$http.adornUrl(`/admin/service/factory/contract-c/deleteById/${id}`),
            method: 'delete',
            // params: this.$http.adornParams()
            }).then( res => {
              // this.dataList = treeDataTranslate(data, 'menuId')
              // this.dataListLoading = false
              this.$message({
              type: 'success',
              message: '删除成功!'
            });
            this.queryProductLIst()
            })

            
          }).catch(() => {
            this.$message({
              type: 'info',
              message: '已取消删除'
            });          
          });
      },

      changeStatus(id , status){
        this.factoryInfo.factoryId = id;
        this.factoryInfo.state = status ? '0' : '1';
        console.log(this.factoryInfo)
        this.$http({
          url: this.$http.adornUrl(`/admin/service/factory/factory-c/updateStatusById`),
          method: 'post',
          // params: this.$http.adornParams()
          }).then( res => {
            // this.dataList = treeDataTranslate(data, 'menuId')
            // this.dataListLoading = false
            this.listinfo = res.data.records
          })
      },
      handleSizeChange(){

      },
      handleCurrentChange(){

      },
        onSubmit() {
          console.log('submit!');
        },
        handleClick(row) {
            console.log(row);
        },
        queryProductLIst(){
          this.$http({
          url: this.$http.adornUrl(`/admin/service/factory/contract-c/queryByCondition/${this.current}/${this.limit}`),
          method: 'post',
          // params: this.$http.adornParams()
          }).then( res => {
            // this.dataList = treeDataTranslate(data, 'menuId')
            // this.dataListLoading = false
            this.listinfo = res.data.rows
          })
        },
    }
}
</script>