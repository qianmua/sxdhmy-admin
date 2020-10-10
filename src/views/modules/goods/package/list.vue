<template>
    <div>
        <h1>货物装箱</h1>
        <el-card class="box-card">
            <el-form :inline="true" class="demo-form-inline">
               
                <el-form-item>
                    <el-button type="primary" @click="removeAll">删除</el-button>
                    <el-button type="primary" @click="updateBatch">修改</el-button>
                    <el-button type="primary" >打印</el-button>
                    
                </el-form-item>
            </el-form>

            <el-table
                :data="listinfo"
                @selection-change="selectChangeEvent"
                style="width: 100%">

                <el-table-column
                type="selection"
                width="55">
                </el-table-column>

                <el-table-column
                    prop="序号"
                    type="index"
                    width="50">
                </el-table-column>
                <el-table-column
                    prop="seller"
                    label="买家"
                    width="300">
                </el-table-column>
                <el-table-column
                    prop="buyer"
                    label="卖家"
                    width="300">
                </el-table-column>
                <el-table-column
                    prop="invoiceNo"
                    label="发票号"
                    width="300">
                </el-table-column>
                <el-table-column
                    prop="invoiceDate"
                    label="发票日期"
                    width="300">
                </el-table-column>
               
                <el-table-column
                    fixed="right"
                    prop="state"
                    width="100"
                    label="状态">
                    <template slot-scope="scope" >
                        <el-tag v-show="scope.row.state == '0'" type="info">草稿</el-tag>
                        <el-tag type="info" v-show="scope.row.state == '1'">已装箱</el-tag>
                    </template>
                </el-table-column>
            </el-table>
        </el-card>

        
        <el-dialog title="修改装箱信息" :visible.sync="dialogFormVisible">
            <el-form :model="updateinfo">
                <el-form-item label="买方">
                    <el-input v-model="updateinfo.seller" autocomplete="off"></el-input>
                </el-form-item>
                <el-form-item label="卖方" >
                    <el-input v-model="updateinfo.buyer" autocomplete="off"></el-input>
                </el-form-item>
                <el-form-item label="发票号" >
                    <el-input v-model="updateinfo.invoiceNo" autocomplete="off"></el-input>
                </el-form-item>
                <el-form-item label="发票日期" >
                    <el-input v-model="updateinfo.invoiceDate" autocomplete="off"></el-input>
                </el-form-item>
                <el-form-item label="唛头" >
                    <el-input v-model="updateinfo.marks" autocomplete="off"></el-input>
                </el-form-item>
                <el-form-item label="描述" >
                    <el-input v-model="updateinfo.descriptions" autocomplete="off"></el-input>
                </el-form-item>
                
            </el-form>
            <div slot="footer" class="dialog-footer">
                <el-button @click="dialogFormVisible = false">取 消</el-button>
                <el-button type="primary" @click="updatePackage">确 定</el-button>
            </div>
        </el-dialog>
    </div>
</template>
<script>
export default {
    data() {
        return {
            dialogFormVisible: false,
            listinfo: [],
            ids: [],
            updateinfo:{
                packingListId: '',
                seller: '',
                exportIds: '',
                buyer: '',
                invoiceNo: '',
                invoiceDate: '',
                marks: '',
                descriptions: '',
                state: 0,
            },
        }
    },
    created(){
        this.queryList()
    },
    methods: {
        updateBatch(){
            
            let idsBatch = []
            let idsAll2 = this.ids
            idsAll2.forEach(v1 => {
                idsBatch.push(v1.packingListId)
            })
            this.$http({
                url: this.$http.adornUrl(`/admin/service/factory/packing-list-c/info/${idsBatch}`),
                method: 'get',
                // data: this.updateinfo,
                }).then( res => {
                    this.updateinfo = res.data.data
                    console.log(this.updateinfo);
                    
            })
            this.dialogFormVisible = true
        },
        selectChangeEvent(rows){
            this.ids = rows
        },
        updatePackage(){
           
            this.$http({
                url: this.$http.adornUrl(`/admin/service/factory/packing-list-c/update`),
                method: 'put',
                data: this.updateinfo,
                }).then( res => {
                    this.$notify({
                    title: '成功',
                    message: '修改成功',
                    type: 'success'
                });
                this.dialogFormVisible = false
                this.queryList()
            })
        },
        removeAll(){
            let idsBatch = []
            let idsAll2 = this.ids
            idsAll2.forEach(v1 => {
                idsBatch.push(v1.packingListId)
            })

            this.$confirm(`此操作将永久删除记录 id:[${idsBatch}], 是否继续?`, '提示', {
            confirmButtonText: '确定',
            cancelButtonText: '取消',
            type: 'warning'
            }).then(() => {
                this.$http({
                url: this.$http.adornUrl(`/admin/service/factory/packing-list-c/removeBatch`),
                method: 'post',
                data:  this.$http.adornData(idsBatch, false),
                }).then( res => {
                this.$message({
                type: 'success',
                message: '删除成功!'
                });
                this.queryList()
            })
            }).catch(() => {
                this.$message({
                type: 'info',
                message: '已取消删除'
                })
            })
        },
        
        queryList(){
            this.$http({
            url: this.$http.adornUrl(`/admin/service/factory/packing-list-c/list`),
            method: 'post',
            // params: this.$http.adornParams()
            }).then( res => {
                // this.dataList = treeDataTranslate(data, 'menuId')
                // this.dataListLoading = false
                this.listinfo = res.data.rows
                
            })
        }
    },
}
</script>