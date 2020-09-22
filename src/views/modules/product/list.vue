<template>

<div>
    <el-form :inline="true" :model="formInline" class="demo-form-inline">
      <el-form-item label="生产厂家">
        <el-input v-model="formInline.user" placeholder="生产厂家"></el-input>
        <!-- <el-autocomplete
          class="inline-input"
          v-model="state1"
          :fetch-suggestions="querySearch"
          placeholder="请输入内容"
          @select="handleSelect"
        ></el-autocomplete> -->
      </el-form-item>
      <el-form-item label="负责人">
        <!-- <el-select v-model="formInline.region" placeholder="活动区域">
          <el-option label="区域一" value="shanghai"></el-option>
          <el-option label="区域二" value="beijing"></el-option>
        </el-select> -->
        <el-input v-model="formInline.user" placeholder="负责人"></el-input>
      </el-form-item>
      <el-form-item label="日期区间">
        <el-date-picker
          v-model="value2"
          type="daterange"
          align="right"
          unlink-panels
          range-separator="至"
          start-placeholder="开始日期"
          end-placeholder="结束日期"
          :picker-options="pickerOptions">
        </el-date-picker>
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
      prop="fullName"
      label="厂家全称"
      width="150">
    </el-table-column>
    <el-table-column
      prop="factoryName"
      label="缩写"
      width="120">
    </el-table-column>
    <el-table-column
      prop="contactor"
      label="联系人"
      width="120">
    </el-table-column>
    <el-table-column
      prop="phone"
      label="电话"
      width="120">
    </el-table-column>
    <el-table-column
      prop="mobile"
      label="手机"
      width="120">
    </el-table-column>
    <el-table-column
      prop="fix"
      label="传真"
      width="120">
    </el-table-column>
    <el-table-column
      prop="inspector"
      label="验货员"
      width="80">
    </el-table-column>
    <el-table-column
      label="是否启用"
      width="150">
      <template slot-scope="scope" >
        <!-- <el-tag v-show="scope.row.state == '0'">启用</el-tag>
        <el-tag type="danger" v-show="scope.row.state != '0'">禁用</el-tag> -->
        <el-switch
          v-model="scope.row.state"
          active-color="#13ce66"
          inactive-color="#ff4949"
          inactive-value="0"
          active-value="1"
          @change="changeStatus(scope.row.factoryId , scope.row.state)"
          >
        </el-switch>
      </template>
    </el-table-column>

    <el-table-column
      fixed="right"
      label="操作"
      width="100">
      <template slot-scope="scope">
        
        <el-button @click="deleteInfo(scope.row.factoryId)" type="text" size="small">删除</el-button>
        <el-button type="text" size="small" @click="updateInfo(scope.row.factoryId)">编辑</el-button>
      </template>
    </el-table-column>
  </el-table>
  <el-pagination
      @size-change="handleSizeChange"
      @current-change="handleCurrentChange"
      :current-page.sync="currentPage2"
      :page-sizes="[5, 10,30,50]"
      :page-size="limit"
      style="text-align:center"
      background
      layout="sizes, prev, pager, next"
      :total="total">
    </el-pagination>

    <add-or-update v-if="addOrUpdateVisible" ref="addOrUpdate" @refreshDataList="getDataList"></add-or-update>


    <el-dialog title="修改厂商信息" :visible.sync="dialogFormVisible">
      <el-form :model="factoryInfo">
        <el-form-item label="厂家名称" prop="name" >
                <el-input v-model="factoryInfo.fullName"></el-input>
            </el-form-item>
            <el-form-item label="联系人" >
                <el-input v-model="factoryInfo.contactor"></el-input>
            </el-form-item>
            <el-form-item label="手机"  >
                <el-input v-model="factoryInfo.mobile"></el-input>
            </el-form-item>
            <el-form-item label="验货员"  >
                <el-input v-model="factoryInfo.inspector"></el-input>
            </el-form-item>
            <el-form-item label="简称"  >
                <el-input v-model="factoryInfo.factoryName"></el-input>
            </el-form-item>
            <el-form-item label="电话"  >
                <el-input v-model="factoryInfo.phone"></el-input>
            </el-form-item>
            <el-form-item label="传真" >
                <el-input v-model="factoryInfo.fax"></el-input>
            </el-form-item>
            <el-form-item label="排序号" >
              <el-input-number v-model="factoryInfo.orderNo"  :min="0"  label="描述文字"></el-input-number>
            </el-form-item>
            
            
            <el-form-item label="启用">
                <el-switch v-model="factoryInfo.state" :inactive-value="0" :active-value="1"></el-switch>
            </el-form-item>
            
            <el-form-item label="备注" prop="desc">
                <el-input type="textarea" v-model="factoryInfo.cnote"></el-input>
            </el-form-item>

      </el-form>
      <div slot="footer" class="dialog-footer">
        <el-button @click="dialogFormVisible = false">取 消</el-button>
        <el-button type="primary" @click="updateInfoById">确 定</el-button>
      </div>
    </el-dialog>
    
</div>
</template>

<script>
import AddOrUpdate from "./config-add-or-update";
export default {
    components: {
      AddOrUpdate,
    },
    data(){
        return {
          dialogFormVisible: false,
          value: true,
          addOrUpdateVisible: false,
           // 条件查询
            formInline: {
              user: '',
              region: ''
              
            },
            currentPage2: 1,
            current: 1 ,
            limit: 5 ,
            total: 0,
            listinfo: [],
            factoryInfo: {
              factoryId: '',
              state : ''
            },
            ids: [],
            pickerOptions: {
              shortcuts: [{
                text: '最近一周',
                onClick(picker) {
                  const end = new Date();
                  const start = new Date();
                  start.setTime(start.getTime() - 3600 * 1000 * 24 * 7);
                  picker.$emit('pick', [start, end]);
                }
              }, {
                text: '最近一个月',
                onClick(picker) {
                  const end = new Date();
                  const start = new Date();
                  start.setTime(start.getTime() - 3600 * 1000 * 24 * 30);
                  picker.$emit('pick', [start, end]);
                }
              }, {
                text: '最近三个月',
                onClick(picker) {
                  const end = new Date();
                  const start = new Date();
                  start.setTime(start.getTime() - 3600 * 1000 * 24 * 90);
                  picker.$emit('pick', [start, end]);
                }
              }]
            },
            value1: '',
            value2: '',

        }
    },
    created(){
        this.queryProductLIst()
    },
    methods:{
      selectChangeEvent(val){
        
        this.ids = val
      },
      deleteAll(){
        let rids = []
        let copysRows = this.ids
        copysRows.forEach(element => {
          rids.push(element.factoryId)
        });
        this.$confirm(`此操作将永久删除记录 id:[${rids}], 是否继续?`, '提示', {
          confirmButtonText: '确定',
          cancelButtonText: '取消',
          type: 'warning'
          }).then(() => {
            this.$http({
            url: this.$http.adornUrl(`/admin/service/factory/factory-c/batchRemove`),
            method: 'post',
            // params: this.$http.adornParams()
            data:  this.$http.adornData(rids, false),
            }).then( res => {
              // this.dataList = treeDataTranslate(data, 'menuId')
              // this.dataListLoading = false
              this.$message({
              type: 'success',
              message: '删除成功!'
            });
            this.queryProductLIst()
          })
      })
            

      },
      addOrUpdate(){
        this.addOrUpdateVisible = true
        this.$nextTick(() => {
          this.$refs.addOrUpdate.init();
        });
      },
      getDataList(){

      },
      handleClose(done) {
        this.$confirm('确认关闭？')
          .then(_ => {
            done();
          })
          .catch(_ => {});
      },
      queryBatch(){

      },
      updateInfoById(){
        this.$http({
          url: this.$http.adornUrl(`/admin/service/factory/factory-c/updateInfo/`),
          method: 'post',
          data: this.factoryInfo,
          }).then( res => {
            this.$message({
              type: 'success',
              message: '删除成功!'
            });
            this.queryProductLIst()
        })

        this.dialogFormVisible = false
      },
      updateInfo(id){
        this.$http({
          url: this.$http.adornUrl(`/admin/service/factory/factory-c/queryById/${id}`),
          method: 'get',
          // data: this.factoryInfo,
          }).then( res => {
            this.factoryInfo = res.data.info
        })

        this.dialogFormVisible = true
        // console.log(id)
        // this.$router.push({path:'./product/add' , params: { id : id}})
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
        this.factoryInfo.factoryId = id
        this.factoryInfo.state = status == '1' ? '1' : '0' 
        console.log(this.factoryInfo.state)
        this.$http({
          url: this.$http.adornUrl(`/admin/service/factory/factory-c/updateStatusById`),
          method: 'post',
          data: this.factoryInfo,
          }).then( res => {
            // this.dataList = treeDataTranslate(data, 'menuId')
            // this.dataListLoading = false
            this.$message({
              type: 'success',
              message: '修改成功'
            });
              this.queryProductLIst()
          })
      },
      handleSizeChange(){
          this.limit = val;
          this.total = 1;
          this.getDataList();
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
        queryById(id){
          this.$http({
          url: this.$http.adornUrl(`/admin/service/factory/factory-c/queryById/${id}`),
          method: 'get',
          // params: this.$http.adornParams()
          }).then( res => {
            // this.dataList = treeDataTranslate(data, 'menuId')
            // this.dataListLoading = false
            console.log(res)
            this.factoryInfo = res.data
          })
        }
    }
}
</script>