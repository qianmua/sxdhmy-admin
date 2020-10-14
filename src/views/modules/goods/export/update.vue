<template>
    <div>
        <h1>出口报运修改</h1>
        <el-form :inline="true"  class="demo-form-inline">
               
                <el-form-item>
                    <el-button type="primary" @click="searchProductList">查询货物列表</el-button>
                </el-form-item>
            </el-form>
        <el-form :model="formUpdate" label-width="80px">


            <el-form-item label="合同号">
                {{this.id}}
            </el-form-item>

            <el-form-item label="信用证号">
                <el-input v-model="formUpdate.lcno"></el-input>
            </el-form-item>
            <el-form-item label="装运量">
                <el-input v-model="formUpdate.cnumber"></el-input>
            </el-form-item>
            <el-form-item label="运输方式">
                <el-radio v-model="formUpdate.transportMode" label="SEA">SEA</el-radio>
                <el-radio v-model="formUpdate.transportMode" label="AIR">AIR</el-radio>
            </el-form-item>
            <el-form-item label="唛头">
                <el-input v-model="formUpdate.marks"></el-input>
            </el-form-item>
            <el-form-item label="制单日期">
                <el-input v-model="formUpdate.createTime"></el-input>
            </el-form-item>
            <el-form-item label="收货人及地址">
                <el-input v-model="formUpdate.consignee"></el-input>
            </el-form-item>
            <el-form-item label="目的地">
                <el-input v-model="formUpdate.destinationPor"></el-input>
            </el-form-item>
            <el-form-item label="价格条件">
                <el-radio v-model="formUpdate.priceCondition" label="FOB">FOB</el-radio>
                <el-radio v-model="formUpdate.priceCondition" label="CIF">CIF</el-radio>
            </el-form-item>
            <el-form-item label="备注">
                <el-input v-model="formUpdate.remark"></el-input>
            </el-form-item>
           
            <el-form-item>
                <el-button type="primary" @click="onSubmit">修改</el-button>
                <el-button>取消</el-button>
            </el-form-item>
        </el-form>


        <!-- card -->
        <el-card class="box-card">
            <el-table
            v-loading="loading"
            :data="tableData"
            style="width: 100%">
            <el-table-column
                prop="productNo"
                label="货号"
                >
            </el-table-column>
            <el-table-column
                prop="cnumber"
                label="数量"
                >
            </el-table-column>
            <el-table-column
                prop="loadingRate"
                label="毛重"
                >
            </el-table-column>
            <el-table-column
                prop="date"
                label="净重"
                >
            </el-table-column>
            <el-table-column
                prop="sizeWidth"
                label="长"
                >
            </el-table-column>
            <el-table-column
                prop="sizeLenght"
                label="宽"
                >
            </el-table-column>
            <el-table-column
                prop="sizeHeight"
                label="高"
                >
            </el-table-column>
            <el-table-column
                prop="price"
                label="出口单价"
                width="80">
            </el-table-column>
            <el-table-column
                prop="tax"
                label="含税"
                width="80">
            </el-table-column>
            
            </el-table>
        </el-card>
    </div>
</template>
<script>
export default {
    data() {
        return {
            loading: true,
            id: '',
            tableData: [],
            formUpdate:{
                exportId: '',
                lcno: '',
                cnumber: ' ',
                transportMode: ' ',
                marks: ' ',
                createTime: ' ',
                consignee: ' ',
                destinationPor: ' ',
                priceCondition: ' ',
                remark: ' ',
            },
        }
    },
    created(){
    },
    mounted(){
        this.id = this.$route.query.id
        this.queryById(this.id)
        this.queryPList(this.id)
    },
    watch:{
        '$route.query.id': function(newVal,oldVal){
            this.id = newVal
            this.queryById(this.id)
            this.formUpdate.exportId = newVal
            this.queryPList(newVal)
        }
    },
    methods: {
        onSubmit(){
            this.$http({
                url: this.$http.adornUrl(`/admin/service/factory/export-c/update`),
                method: 'put',
                data: this.formUpdate,
                }).then( res => {
                this.$notify({
                    title: '成功',
                    message: '修改成功',
                    type: 'success'
                });
                let t = (new Date()).valueOf()
                
                this.$router.push({path:'/goods-export/list',query:{t,t}})
            })
        },
        queryById(id){
            
            this.$http({
            url: this.$http.adornUrl(`/admin/service/factory/export-c/info/${id}`),
            method: 'get',
            // params: this.$http.adornParams()
            }).then( res => {
                // this.dataList = treeDataTranslate(data, 'menuId')
                // this.dataListLoading = false
                this.formUpdate = res.data.data
  
            })
        },
        searchProductList(){
            
            this.queryPList(this.id)
        },
        queryPList(id){
            this.$http({
            url: this.$http.adornUrl(`/admin/service/factory/export-product-c/list/${id}`),
            method: 'get',
            // params: this.$http.adornParams()
            }).then( res => {
                // this.dataList = treeDataTranslate(data, 'menuId')
                // this.dataListLoading = false
                this.tableData = res.data.rows
                this.loading = false
                console.log(this.tableData)
                
            })
        },
    },
}
</script>