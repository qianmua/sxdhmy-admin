<template>
    <div>

        <el-form :model="ruleForm" label-width="100px" class="demo-ruleForm">
            <el-form-item label="厂家名称" prop="name" >
                <el-input v-model="ruleForm.fullName"></el-input>
            </el-form-item>
            <el-form-item label="联系人" >
                <el-input v-model="ruleForm.contactor"></el-input>
            </el-form-item>
            <el-form-item label="手机"  >
                <el-input v-model="ruleForm.mobile"></el-input>
            </el-form-item>
            <el-form-item label="验货员"  >
                <el-input v-model="ruleForm.inspector"></el-input>
            </el-form-item>
            <el-form-item label="简称"  >
                <el-input v-model="ruleForm.factoryName"></el-input>
            </el-form-item>
            <el-form-item label="电话"  >
                <el-input v-model="ruleForm.phone"></el-input>
            </el-form-item>
            <el-form-item label="传真" >
                <el-input v-model="ruleForm.fax"></el-input>
            </el-form-item>
            <el-form-item label="排序号" >
              <el-input-number v-model="ruleForm.orderNo"  :min="0"  label="描述文字"></el-input-number>
            </el-form-item>
            
            
            <el-form-item label="启用">
                <el-switch v-model="ruleForm.state" :inactive-value="0" :active-value="1"></el-switch>
            </el-form-item>
            
            <el-form-item label="备注" prop="desc">
                <el-input type="textarea" v-model="ruleForm.cnote"></el-input>
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
        num: 0 ,
        ruleForm: {
          fullName: '',
          contactor: '',
          mobile: '',
          inspector: '',
          factoryName: '',
          phone: '',
          fax: '',
          orderNo: 0,
          cnote: '',
          state: '1'
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
        console.log(this.ruleForm.state)
            this.$http({
            url: this.$http.adornUrl(`/admin/service/factory/factory-c/add`),
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
        this.ruleForm.fullName = ''
        this.ruleForm.contactor = ''
        this.ruleForm.mobile = ''
        this.ruleForm.inspector = ''
        this.ruleForm.factoryName = ''
        this.ruleForm.phone = ''
        this.ruleForm.fax = ''
        this.ruleForm.orderNo = 0
        this.ruleForm.cnote = ''
        
        this.ruleForm.state = '1'
      }
    }
  }
</script>