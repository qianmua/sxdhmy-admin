<template>
    <div>
        <el-form :model="numberValidateForm" label-width="100px" class="demo-ruleForm">
          <el-form-item label="打印版式">
            <el-radio v-model="numberValidateForm.type" label="1">一版</el-radio>
            <el-radio v-model="numberValidateForm.type" label="2">两版</el-radio>
          </el-form-item>
            <el-form-item
                label="日期"
            >
                <el-date-picker
                v-model="numberValidateForm.time"
                type="date"
                value-format="yyyy-MM-dd"
                placeholder="选择日期">
                </el-date-picker>
            </el-form-item>
            <el-form-item>
                <el-button type="primary" @click="submitForm()">提交</el-button>
            </el-form-item>
        </el-form>
    </div>
</template>
<script>
  export default {
    data() {
      return {
        numberValidateForm: {
          age: '',
          time: '',
          type: 0,
        }
      };
    },
    methods: {
      submitForm() {
        // 创建a标签 
      // let link = document.createElement('a')
      // href链接 
      // link.setAttribute('href', row.contract_file_url)
      // 自执行点击事件
      // link.click()
      console.log(this.numberValidateForm.time);
      
      this.$http({
          url: this.$http.adornUrl(`/ofs/table/option/gen/excel`),
          method: 'post',
          data: this.numberValidateForm.time,
          }).then( res => {
            let blob = new Blob([res], {type: "application/vnd.ms-excel"}); //res 就是文件流了
            let objectUrl = URL.createObjectURL(blob);
            window.location.href = objectUrl
        })

      },
      // resetForm(formName) {
      //   this.$refs[formName].resetFields();
      // }
    }
  }
</script>