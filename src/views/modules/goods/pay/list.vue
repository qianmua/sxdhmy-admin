<template>
    <div>

        <el-form :inline="true" :model="formInline" class="demo-form-inline">
      <el-form-item label="审批人">
        <el-input v-model="formInline.user" placeholder="审批人"></el-input>
      </el-form-item>
      <el-form-item label="活动区域">
        <el-select v-model="formInline.region" placeholder="活动区域">
          <el-option label="区域一" value="shanghai"></el-option>
          <el-option label="区域二" value="beijing"></el-option>
        </el-select>
      </el-form-item>
      <el-form-item>
        <el-button type="primary" @click="onSubmit">查询</el-button>
        <el-button type="primary" @click="queryBatch">精确查询</el-button>
      </el-form-item>
    </el-form>
    <el-table
    :data="listinfo"
    border
    style="width: 100%">
    <el-table-column
      fixed
      type="index"
      label="序号"
      width="100">
    </el-table-column>
    <el-table-column
      prop="contactor"
      label="客户名称"
      width="120">
    </el-table-column>
    <el-table-column
      prop="ctype"
      label="合同号"
      width="120">
    </el-table-column>
    <el-table-column
      prop="mobile"
      label="货物数/附件数"
      width="120">
    </el-table-column>
    <el-table-column
      prop="orderNo"
      label="制单人"
      width="80">
    </el-table-column>
    <el-table-column
      prop="phone"
      label="审核人"
      width="120">
    </el-table-column>
    <el-table-column
      prop="createTime"
      label="验货员"
      width="100">
    </el-table-column>
    <el-table-column
      prop="createTime"
      label="签单日期"
      width="200">
    </el-table-column>
    <el-table-column
      prop="createTime"
      label="交货期限"
      width="200">
    </el-table-column>
    <el-table-column
      prop="createTime"
      label="船期"
      width="200">
    </el-table-column>
    <el-table-column
      prop="createTime"
      label="总金额"
      width="120">
    </el-table-column>
    
    <el-table-column
      prop="state"
      label="状态"
      width="150">
      <template slot-scope="scope" >
        <el-tag v-show="scope.row.state == '0'">启用</el-tag>
        <el-tag type="danger" v-show="scope.row.state != '0'">禁用</el-tag>
        <!-- <el-switch
          style="display: block"
          :v-model="scope.row.state"
          active-color="#13ce66"
          inactive-color="#ff4949"
          active-text="启用"
          inactive-text="禁用"
          @change="changeStatus(scope.row.factoryId , scope.row.state)"
          >
        </el-switch> -->
      </template>
    </el-table-column>

    <el-table-column
      fixed="right"
      label="操作"
      width="100">
      <template slot-scope="scope">
        
        <el-button @click="deleteInfo(scope.row.factoryId)" type="text" size="small">删除</el-button>
        <el-button type="text" size="small" @click="updateInfo">编辑</el-button>
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
      handleClose(done) {
        this.$confirm('确认关闭？')
          .then(_ => {
            done();
          })
          .catch(_ => {});
      },
      queryBatch(){
        
      },
      updateInfo(){

      },
      deleteInfo(id){
        //deleteById
          this.$confirm(`此操作将永久删除记录 id:[${id}], 是否继续?`, '提示', {
          confirmButtonText: '确定',
          cancelButtonText: '取消',
          type: 'warning'
          }).then(() => {
            this.$http({
            url: this.$http.adornUrl(`/admin/service/factory/factory-c/deleteById/${id}`),
            method: 'delete',
            // params: this.$http.adornParams()
            }).then( res => {
              // this.dataList = treeDataTranslate(data, 'menuId')
              // this.dataListLoading = false
              this.$message({
              type: 'success',
              message: '删除成功!'
            });
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
            console.log(res.data.records)
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
          url: this.$http.adornUrl(`/admin/service/factory/factory-c/queryByCondition/${this.current}/${this.limit}`),
          method: 'post',
          // params: this.$http.adornParams()
          }).then( res => {
            // this.dataList = treeDataTranslate(data, 'menuId')
            // this.dataListLoading = false
            console.log(res.data.records)
            this.listinfo = res.data.records
          })
        },
    }
}
</script>