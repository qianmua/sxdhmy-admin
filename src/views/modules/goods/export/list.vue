<template>
    <div>
        <h1>货物报运</h1>
        <el-card class="box-card">
            <el-form :inline="true" :model="formInline" class="demo-form-inline">
               
                <el-form-item>
                    <el-button type="primary" >上报</el-button>
                    <el-button type="primary" @click="removeAll">删除</el-button>
                    <el-button type="primary" @click="updateBatch">修改</el-button>
                    
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
                    prop="customerContract"
                    label="合同号或确认书号"
                    width="300">
                </el-table-column>
                <el-table-column
                    prop="lcno"
                    label="信用证号">
                </el-table-column>
                <el-table-column
                    width="150"
                    label="货物数/附件数">
                    <template slot-scope="scope">
                        {{ scope.row.number}} / {{ scope.row.number2}}
                    </template>

                </el-table-column>
                <el-table-column
                    prop="consignee"
                    width="150"
                    label="收货人及地址">
                </el-table-column>
                <el-table-column
                    prop="transportMode"
                    label="装运港">
                </el-table-column>
                <el-table-column
                    prop="destinationPor"
                    label="目的港">
                </el-table-column>
                <el-table-column
                    prop="address"
                    label="运输方式">
                </el-table-column>
                <el-table-column
                    prop="priceCondition"
                    label="价格条件">
                </el-table-column>
                <el-table-column
                    prop="createTime"
                    width="180"
                    label="制单日期">
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
    </div>
</template>
<script>
export default {
    data() {
        return {
            formInline:{},
            listinfo:[],
            ids:[],
        }
    },
    created(){
        this.queryAllExport()
    },
    methods: {
        removeAll(){
            let idsBatch = []
            let idsAll2 = this.ids
            idsAll2.forEach(v1 => {
                idsBatch.push(v1.exportId)
            })

            this.$confirm(`此操作将永久删除记录 id:[${idsBatch}], 是否继续?`, '提示', {
            confirmButtonText: '确定',
            cancelButtonText: '取消',
            type: 'warning'
            }).then(() => {
                this.$http({
                url: this.$http.adornUrl(`/admin/service/factory/export-c/removeBatch`),
                method: 'post',
                data:  this.$http.adornData(idsBatch, false),
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
        updateBatch(){

        },
        selectChangeEvent(rows){
            this.ids = rows
        },
        queryAllExport(){
            this.$http({
            url: this.$http.adornUrl(`/admin/service/factory/export-c/list`),
            method: 'post',
            // params: this.$http.adornParams()
            }).then( res => {
                // this.dataList = treeDataTranslate(data, 'menuId')
                // this.dataListLoading = false
                this.listinfo = res.data.rows
                console.log(this.listinfo);
                
            })

        },
    },
}
</script>