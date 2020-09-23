<template>
    <div>
        

        <el-form :model="ruleForm"  label-width="100px" class="demo-ruleForm">
            
            <el-form-item label="客户名称" prop="name">
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
                <el-select v-model="selAll" clearable placeholder="请选择">
                <el-option
                  v-for="item in options"
                  :key="item.value"
                  :label="item.label"
                  :value="item.value">
                </el-option>
              </el-select>
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

            <el-form-item>
                <el-button type="primary" @click="submitForm('ruleForm')">添加</el-button>
                <el-button @click="resetForm">重置</el-button>
            </el-form-item>
        </el-form>
    </div>
</template>
<script>
  export default {
    data() {
      return {
        selAll: '',
        options: [{
          value: '1',
          label: '条款A'
        }, {
          value: '2',
          label: '条款B'
        }, {
          value: '3',
          label: '条款C'
        }, {
          value: '4',
          label: '条款D'
        }, {
          value: '5',
          label: '条款E'
        }],
        num: 0 ,
        ruleForm: {
            checkBy: "",
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
        },
        rules: {
          name: [
            { required: true, message: '请输入厂商名称', trigger: 'blur' },
            { min: 3, max: 20, message: '长度在 3 到 5 个字符', trigger: 'blur' }
          ],
          
          // date1: [
          //   { type: 'date', required: true, message: '请选择日期', trigger: 'change' }
          // ],
          // date2: [
          //   { type: 'date', required: true, message: '请选择时间', trigger: 'change' }
          // ],
          // type: [
          //   { type: 'array', required: true, message: '请至少选择一个活动性质', trigger: 'change' }
          // ],
          // resource: [
          //   { required: true, message: '请选择活动资源', trigger: 'change' }
          // ],
          desc: [
            { required: true, message: '请填写内容', trigger: 'blur' }
          ]
        }
      };
    },
    created(){
      
    },
    methods: {
      submitForm(formName) {
            this.$http({
            url: this.$http.adornUrl(`/admin/service/factory/contract-c/addInfo`),
            method: 'put',
            data: this.ruleForm,
            }).then( res => {
              this.$notify({
              title: '成功',
              message: '添加成功',
              type: 'success'
            });
            this.resetForm()
            })
      },
      resetForm() {
        this.ruleForm.checkBy=""
        this.ruleForm.contractId= ""
        this.ruleForm.contractNo= ""
        this.ruleForm.createBy= ""
        this.ruleForm.createDept= ""
        this.ruleForm.createTime= ""
        this.ruleForm.customName= ""
        this.ruleForm.deliveryPeriod= ""
        this.ruleForm.importNum= 0
        this.ruleForm.inputBy=""
        this.ruleForm.inspector= ""
        this.ruleForm.offeror= ""
        this.ruleForm.oldState= 0
        this.ruleForm.outState= 0
        this.ruleForm.printStyle= 0
        this.ruleForm.remark= ""
        this.ruleForm.request= ""
        this.ruleForm.shipTime= ""
        this.ruleForm.signingDate= ""
        this.ruleForm.state= 0
        this.ruleForm.totalAmount= 0
      }
    }
  }
</script>