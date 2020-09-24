<template>
    <div>
        <el-row :gutter="20">
            <el-col :span="6">
                <el-card class="box-card">
                    <div slot="header" class="clearfix">
                        <span>附件菜单</span>
                        <el-button style="float: right; padding: 3px 0" type="text" @click="addInfo">添加附件</el-button>
                    </div>

                    <el-table
                    :data="list"
                    @row-click="clievent"
                    style="width: 100%">
                    <el-table-column
                        prop="factory"
                        label="厂家名称"
                        width="80">
                    </el-table-column>
                    <el-table-column
                        prop="productNo"
                        label="货号"
                        width="80">
                    </el-table-column>
                    <el-table-column
                        fixed="right"
                        label="操作">
                        <template slot-scope="scope">
                            <el-button @click="deleteBy(scope.row.extCproductId)" type="text" size="small">删除</el-button>
                        </template>
                    </el-table-column>
                    </el-table>
            </el-card>
            </el-col>
            <el-col :span="18">
                <el-card class="box-card">
                    <div slot="header" class="clearfix">
                        <span>货物明细</span>
                        <el-button style="float: right; padding: 3px 0" type="text" @click="updateInfo">修改附件信息</el-button>
                    </div>
                    
                    <el-row :gutter="20">
                        <el-col :span="10"><div class="grid-content bg-purple">厂家名称：{{dataInfo.factory}}</div></el-col>
                        <el-col :span="10"><div class="grid-content bg-purple">货号：{{dataInfo.productNo}}</div></el-col>
                    </el-row>
                    <el-row :gutter="20">
                        <el-col :span="10"><div class="grid-content bg-purple">数量：{{dataInfo.extCnumber}}</div></el-col>
                        <el-col :span="10"><div class="grid-content bg-purple">包装单位：{{dataInfo.packingUnit}}</div></el-col>
                    </el-row>
                    <el-row :gutter="20">
                        <el-col :span="10"><div class="grid-content bg-purple">单价：{{dataInfo.price}}</div></el-col>
                        <el-col :span="10"><div class="grid-content bg-purple">总金额：{{dataInfo.price *dataInfo.extCnumber }}</div></el-col>
                    </el-row>
                    <!-- <el-row :gutter="20">
                        <el-col :span="10"><div class="grid-content bg-purple">货号：{{dataInfo.boxNum}}</div></el-col>
                        <el-col :span="10"><div class="grid-content bg-purple">货物照片：{{dataInfo.price}}</div></el-col>
                    </el-row>
                    <el-row :gutter="20">
                        <el-col :span="10"><div class="grid-content bg-purple">包装单位：{{dataInfo.price * dataInfo.cnumber }}</div></el-col>
                        <el-col :span="10"><div class="grid-content bg-purple">要求：{{dataInfo.cnumber}}</div></el-col>
                    </el-row> -->
                </el-card>

            </el-col>
         
        </el-row>

        <!-- ////////////////// -->
        <el-dialog title="添加附件信息" :visible.sync="dialogFormVisible" width="900px">
            <el-form :model="dataInfo"  label-width="100px">
                <el-row :gutter="20">
                    <el-col :span="12">
                        <el-form-item label="厂家名称" >
                            <el-select v-model="dataInfo.factoryId" placeholder="请选择">
                                <el-option
                                v-for="item in factorys"
                                :key="item.factoryId"
                                :label="item.factoryName"
                                :value="item.factoryId">
                                </el-option>
                            </el-select>
                        </el-form-item>
                        <el-form-item label="分类" >
                            <el-select v-model="dataInfo.ctype" placeholder="请选择">
                                <el-option
                                v-for="item in sysList"
                                :key="item.sysCodeId"
                                :label="item.name"
                                :value="item.sysCodeId">
                                </el-option>
                            </el-select>
                        </el-form-item>
                        <el-form-item label="数量" >
                            <el-input-number v-model="dataInfo.extCnumber" :min="1" :max="999999" label="数量"></el-input-number>
                        </el-form-item>
                        <el-form-item label="单价" >
                        <el-input v-model="dataInfo.price" autocomplete="off"></el-input>
                        </el-form-item>
                        
                        <el-form-item label="货物描述" >
                            <el-input
                            type="textarea"
                            :rows="4"
                            placeholder="请输入内容"
                            v-model="dataInfo.productDesc">
                            </el-input>
                        </el-form-item>
                    </el-col>
                    <el-col :span="12">
                        <el-form-item label="货号" >
                        <el-input v-model="dataInfo.productNo" autocomplete="off"></el-input>
                        </el-form-item>
                        <el-form-item label="货物照片" >
                        <el-input v-model="dataInfo.productImage" autocomplete="off"></el-input>
                        </el-form-item>
                        <el-form-item label="包装单位" >
                        <el-input v-model="dataInfo.packingUnit" autocomplete="off"></el-input>
                        </el-form-item>
                        <el-form-item label="排序号" >
                            <el-input-number v-model="dataInfo.orderNo" :min="1" :max="999999" label="描述文字"></el-input-number>
                        </el-form-item>
                        <el-form-item label="要求" >
                            <el-input
                            type="textarea"
                            :rows="4"
                            placeholder="请输入内容"
                            v-model="dataInfo.productRequest">
                            </el-input>
                        </el-form-item>
                    </el-col>
                </el-row>
                
            </el-form>
            <div slot="footer" class="dialog-footer">
                <el-button @click="closedio">取 消</el-button>
                <el-button type="primary" @click="addData">确 定</el-button>
            </div>
        </el-dialog>
        <el-dialog title="请选择货物编号" :visible.sync="dialogFormVisible2" width="300px">
        <el-form >
            <el-form-item label="编号">
                <el-select v-model="dataInfo.contractProductId" placeholder="请选择">
                    <el-option
                    v-for="item in contList"
                    :key="item.contractProductId"
                    :label="item.productNo"
                    :value="item.contractProductId">
                    </el-option>
                </el-select>
            </el-form-item>
        </el-form>
        <div slot="footer" class="dialog-footer">
            <el-button @click="dialogFormVisible2 = false">取 消</el-button>
            <el-button type="primary" @click="isinputValue">确 定</el-button>
        </div>
        </el-dialog>
    </div>
</template>
<script>
export default {
    data(){
        return{
            dialogFormVisible: false,
            dialogFormVisible2: false,
            dataInfo: {
                accessories: 0,
                amount: 0,
                boxNum: 0,
                contractProductId: "",
                costPrice: 0,
                costTax: 0,
                ctype: 0,
                cunit: "",
                exPrice: 0,
                exUnit: "",
                extCnumber: 0,
                extCproductId: "",
                factory: "",
                factoryId: "",
                finished: 0,
                grossWeigh: 0,
                loadingRate: "",
                netWeight: 0,
                noTax: 0,
                orderNo: 0,
                outNumber: 0,
                packingUnit: "",
                price: 0,
                productDesc: "",
                productImage: "",
                productName: "",
                productNo: "",
                productRequest: "",
                sizeHeight: 0,
                sizeLenght: 0,
                sizeWidth: 0,
                tax: 0

            },
            list:[],
            factorys: [],
            contList: [],
            sysList: [],
            num: 0,
            current: 1,
            limit: 1000,
            isAdd: false,
            addOrUpdate : true
        }
    },
    created(){
        let id = this.$route.query.id
        this.baseId = id
        if(id != undefined){
            this.queryProductLIst2()
            this.queryList(id)
        }else{
            
            this.showDoalog()
        }
        this.queryFactoryList()
        this.querySysList()
    },
    mounted(){
        let id = this.$route.query.id
        this.baseId = id
        if(id != undefined){
            this.queryList(id)
            
        }else{
            this.queryProductLIst2()
            this.showDoalog()
        }
        this.queryFactoryList()
        this.querySysList()
    },
    watch:{
        '$route.query.id': function(newVal,oldVal){
            this.queryList(newVal)
        }
    },
    methods:{
        selectId(){
        },
        showDoalog(){
            this.dialogFormVisible2 = true
        },

        addInfo(){
            let id = this.$route.query.id
            this.isAdd = true
            
            if(id === undefined && this.dataInfo.contractProductId.length === 0){
                this.dialogFormVisible2 = true
            }else{
                this.dialogFormVisible = true
            }
        },
        clievent(row){
            this.queryById(row.extCproductId)
        },
        isinputValue(){
            if(this.dataInfo.contractProductId.length < 1){
                this.queryProductLIst2()
                this.dialogFormVisible2 = true
            }else{
                this.dialogFormVisible2 = false
                this.baseId = this.dataInfo.contractProductId
                this.queryList(this.dataInfo.contractProductId)
                if(this.isAdd){
                    this.dialogFormVisible = true
                }
            }
        },
        queryList(id){
            this.$http({
            url: this.$http.adornUrl(`/admin/service/factory/ext-cproduct-c/queryAll/${id}/${this.current}/${this.limit}`),
            method: 'post',
            }).then( res => {
                this.list = res.data.rows
            })
        },
        queryById(id){
            this.$http({
            url: this.$http.adornUrl(`/admin/service/factory/ext-cproduct-c/queryById/${id}`),
            method: 'get',
            // data: this.dataInfo,
            }).then( res => {
              this.dataInfo = res.data.info
              
            })
        
        },
        updateInfo2(){
            let facArr = this.factorys
            facArr.forEach(v1 =>{
                if(v1.factoryId == this.dataInfo.factoryId ){
                    this.dataInfo.factory = v1.factoryName
                }
            })
            this.$http({
            url: this.$http.adornUrl(`/admin/service/factory/ext-cproduct-c/updateInfo`),
            method: 'put',
            data: this.dataInfo,
            }).then( res => {

              this.$notify({
              title: '成功',
              message: '修改',
              type: 'success'
            });
            this.dialogFormVisible = false
            this.queryList(this.baseId)
            })
        },
        closedio(){
          this.dialogFormVisible = false  
          this.addOrUpdate = true
        },
        updateInfo(){
            this.addOrUpdate = false
            let id = this.dataInfo.contractProductId
            //
            //
            //
            //
            if(id.length > 0){
                // this.queryById(id)
                this.dialogFormVisible = true
            }else{
                this.$message({
                    type: 'info',
                    message: '请选择左侧附件清单'
                })
            }            
        },
        add(){
            let facArr = this.factorys
            facArr.forEach(v1 =>{
                if(v1.factoryId == this.dataInfo.factoryId ){
                    this.dataInfo.factory = v1.factoryName
                }
            })
            this.dataInfo.extCproductId = ''
            this.dataInfo.contractProductId = this.baseId
            console.log("id: " + this.baseId);
            
            this.$http({
            url: this.$http.adornUrl(`/admin/service/factory/ext-cproduct-c/addInfo`),
            method: 'post',
            data: this.dataInfo,
            }).then( res => {
              this.$notify({
              title: '成功',
              message: '添加成功',
              type: 'success'
            });
            this.queryList(this.baseId)
            this.dialogFormVisible = false
            })
        },
        addData(){
            
            if(this.addOrUpdate){
                
                this.add()
                this.addOrUpdate = true
            }else{
                this.updateInfo2()
                this.addOrUpdate = true
            }
        },
        deleteBy(id){
            this.$confirm(`此操作将永久删除记录 id:[${id}], 是否继续?`, '提示', {
          confirmButtonText: '确定',
          cancelButtonText: '取消',
          type: 'warning'
          }).then(() => {
            this.$http({
            url: this.$http.adornUrl(`/admin/service/factory/ext-cproduct-c/deleteById/${id}`),
            method: 'delete',
            // params: this.$http.adornParams()
            }).then( res => {
              // this.dataList = treeDataTranslate(data, 'menuId')
              // this.dataListLoading = false
              this.$message({
              type: 'success',
              message: '删除成功!'
            });
            this.queryList(this.dataInfo.contractProductId)
            })

            
          }).catch(() => {
            this.$message({
              type: 'info',
              message: '已取消删除'
            })          
          });
        },
        clearInfo(){

        },
        queryFactoryList(){
            this.$http({
          url: this.$http.adornUrl(`/admin/service/factory/factory-c/queryByCondition/${this.current}/${this.limit}`),
          method: 'post',
          }).then( res => {
            this.factorys = res.data.records
          })
        },
        queryProductLIst2(){
          this.$http({
          url: this.$http.adornUrl(`/admin/service/factory/contract-product-c/queryAllInfo/${this.current}/${this.limit}`),
          method: 'post',
          }).then( res => {
              
            this.contList = res.data.rows.records
          })
        },
        querySysList(){
            this.$http({
          url: this.$http.adornUrl(`/admin/service/factory/sys-code-b/queryAll/${this.current}/${this.limit}`),
          method: 'post',
          }).then( res => {
            this.sysList = res.data.rows
          })
        },

    }
}
</script>


<style>
  .el-row {
    margin-bottom: 20px;
  }
  .el-col {
    border-radius: 4px;
  }
  .bg-purple-dark {
    background: #99a9bf;
  }
  .bg-purple {
    background: #d3dce6;
  }
  .bg-purple-light {
    background: #e5e9f2;
  }
  .grid-content {
    border-radius: 4px;
    min-height: 36px;
  }
  .row-bg {
    padding: 10px 0;
    background-color: #f9fafc;
  }
</style>