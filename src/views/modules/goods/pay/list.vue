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
        
        <el-button type="primary" @click="updateStatusAll">上报</el-button>
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

    <el-table-column type="expand">
      <template slot-scope="props">
        <el-form label-position="left" :inline="true" class="demo-table-expand">
          <el-form-item label="客户名称">
            <span>{{ props.row.contractNo }}</span>
          </el-form-item>
          <el-form-item label="合同号">
            <span>{{ props.row.contractNo }}</span>
          </el-form-item>
          <el-form-item label="签单日期">
            <span>{{ props.row.contractNo }}</span>
          </el-form-item>
          <el-form-item label="交货期限">
            <span>{{ props.row.contractNo }}</span>
          </el-form-item>
          <el-form-item label="贸易条款">
            <span>{{ props.row.contractNo }}</span>
          </el-form-item>
          <el-form-item label="制单人">
            <span>{{ props.row.contractNo }}</span>
          </el-form-item>
          <el-form-item label="要求">
            <span>{{ props.row.contractNo }}</span>
          </el-form-item>
          <br>
          <el-form-item label="要求">
            <span>{{ props.row.contractNo }}</span>
          </el-form-item>
          <el-form-item label="要求">
            <span>{{ props.row.contractNo }}</span>
          </el-form-item>
          <el-form-item label="要求">
            <span>{{ props.row.contractNo }}</span>
          </el-form-item>
          <el-form-item label="要求">
            <span>{{ props.row.contractNo }}</span>
          </el-form-item>

        </el-form>

      </template>
    </el-table-column>

    <el-table-column
      fixed
      type="index"
      label="序号"
      width="70">
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
      width="100">
      <template slot-scope="scope" >
        <el-tag v-show="scope.row.state == '0'">未走货</el-tag>
        <el-tag type="info" v-show="scope.row.state == '1'">草稿</el-tag>
        <el-tag type="success" v-show="scope.row.state == '2'">已上报</el-tag>
      </template>
    </el-table-column>
    <el-table-column
      fixed="right"
      label="操作"
      width="150">
      <template slot-scope="scope">
        
        <el-button  type="text" size="small" @click="shop(scope.row.contractId)" >货物</el-button>
        <el-button type="text" size="small" @click="updateInfo(scope.row.contractId)">修改</el-button>
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


  <el-dialog title="修改合同信息" :visible.sync="dialogFormVisible">
      <el-form :model="ruleForm"  label-width="100px" class="demo-ruleForm">
            
            <el-form-item label="客户名称">
                <el-input v-model="ruleForm.customName"  ></el-input>
            </el-form-item>
            <el-form-item label="合同号" >
                <el-input v-model="ruleForm.contractNo" ></el-input>
            </el-form-item>
            <el-form-item label="签单日期"  >
                <el-date-picker
                value-format="yyyy-MM-dd"

                v-model="ruleForm.signingDate"
                type="date"
                placeholder="选择日期">
                </el-date-picker>
            </el-form-item>
            <el-form-item label="交货期限"  >
                <el-date-picker
                v-model="ruleForm.deliveryPeriod"
                value-format="yyyy-MM-dd"
                type="date"
                placeholder="选择日期">
                </el-date-picker>
            </el-form-item>
            <el-form-item label="贸易条款"  >
                <el-input ></el-input>
            </el-form-item>
            <el-form-item label="制单人"  >
                <el-input v-model="ruleForm.inputBy"></el-input>
            </el-form-item>
            <el-form-item label="要求" >
                <el-input type="textarea" v-model="ruleForm.request"></el-input>
            </el-form-item>


            <el-form-item label="收购方" >
              <el-input v-model="ruleForm.offeror"></el-input>
            </el-form-item>

            <el-form-item label="打印版式" >
              <el-radio v-model="ruleForm.printStyle" :label="1" border>两款</el-radio>
             <el-radio v-model="ruleForm.printStyle" :label="2" border>一款</el-radio>
            </el-form-item>
            <el-form-item label="重要程度" >
              <el-radio v-model="ruleForm.importNum" :label="3" border>⭐⭐⭐</el-radio>
             <el-radio v-model="ruleForm.importNum" :label="2" border>⭐⭐</el-radio>
             <el-radio v-model="ruleForm.importNum" :label="1" border>⭐</el-radio>
            </el-form-item>

            <el-form-item label="船期" >
              <el-date-picker
                v-model="ruleForm.shipTime"
                type="date"
                value-format="yyyy-MM-dd"
                placeholder="选择日期">
                </el-date-picker>
            </el-form-item>
            <el-form-item label="验货员">
                <el-input v-model="ruleForm.inspector"></el-input>
            </el-form-item>
            <el-form-item label="审单人">
                <el-input v-model="ruleForm.checkBy"></el-input>
            </el-form-item>


            <el-form-item label="说明" prop="desc">
                <el-input type="textarea" v-model="ruleForm.remark"></el-input>
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
export default {
  data(){
        return {
          dialogFormVisible: false,
          ruleForm: {
            checkBy: "",
            contractId: "",
            contractNo: "",
            createBy: "",
            createDept: "",
            createTime: "",
            customName: "",
            deliveryPeriod: "",
            importNum: 1,
            inputBy: "",
            inspector: "",
            offeror: "",
            oldState: 0,
            outState: 0,
            printStyle: 1,
            remark: "",
            request: "",
            shipTime: "",
            signingDate: "",
            state: 0,
            totalAmount: 0,
            idsAll:[],
          },
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
      shop(id){
        this.$router.push({path:'/goods-shop/add' , query:{id:id} })
      },

      selectChangeEvent(rows){
        this.idsAll = rows
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
      updateStatusAll(){
        let ids = []
        let idsAll2 = this.idsAll
        idsAll2.forEach(v1 => {
          ids.push(v1.contractId)
        })

        this.$confirm(`此操作有危险, 是否继续?`, '提示', {
          confirmButtonText: '确定',
          cancelButtonText: '取消',
          type: 'warning'
          }).then(() => {
            this.$http({
            url: this.$http.adornUrl(`/admin/service/factory/contract-c/updateInfo`),
            method: 'post',
            data:  this.$http.adornData(ids, false),
            }).then( res => {
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
            })
        })

      },
      deleteAll(){
        let ids = []
        let idsAll2 = this.idsAll
        idsAll2.forEach(v1 => {
          ids.push(v1.contractId)
        })

        this.$confirm(`此操作将永久删除记录 id:[${ids}], 是否继续?`, '提示', {
          confirmButtonText: '确定',
          cancelButtonText: '取消',
          type: 'warning'
          }).then(() => {
            this.$http({
            url: this.$http.adornUrl(`/admin/service/factory/contract-c/deleteBatch`),
            method: 'post',
            data:  this.$http.adornData(ids, false),
            }).then( res => {
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
            })
        })
        
      },
      queryBatch(){
        
      },
      nothings(){

      },
      updateInfo(id){
        this.dialogFormVisible = true
        this.queryById(id)
      },
      updateInfoById(){
          this.$http({
          url: this.$http.adornUrl(`/admin/service/factory/contract-c/updateInfo/`),
          method: 'post',
          data: this.ruleForm,
          }).then( res => {
            this.$message({
              type: 'success',
              message: '修改成功!'
            });
            this.queryProductLIst()
        })

        this.dialogFormVisible = false
      },
      deleteInfo(id){
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
            })          
          });
      },
      //update
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
        queryById(id){
          this.$http({
          url: this.$http.adornUrl(`/admin/service/factory/contract-c/queryById/${id}`),
          method: 'get',
          // params: this.$http.adornParams()
          }).then( res => {
            // this.dataList = treeDataTranslate(data, 'menuId')
            // this.dataListLoading = false
            console.log(res.data.info)
            this.ruleForm = res.data.info
          })
        }
    }
}
</script>