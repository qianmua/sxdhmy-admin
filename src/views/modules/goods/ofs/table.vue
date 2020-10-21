<template>
    <div>
      <h1>货物打印</h1>
        <el-form :model="numberValidateForm" label-width="100px" class="demo-ruleForm">
          <el-form-item label="打印版式">
            <el-radio v-model="numberValidateForm.type" :label="0">一版</el-radio>
            <el-radio v-model="numberValidateForm.type" :label="1">两版</el-radio>
          </el-form-item>
            <el-form-item
                label="日期"
            >
                <el-date-picker
                v-model="numberValidateForm.time"
                type="month"
                value-format="yyyy-MM-dd"
                placeholder="选择日期">
                </el-date-picker>
            </el-form-item>
            <el-form-item>
                <!-- <el-button type="primary" @click="submitForm()">提交</el-button> -->
                <el-button type="primary" @click="submitForm2()">提交</el-button>
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
      // link.setAttribute('href', '/ofs/table/option/test')
      // 自执行点击事件
      // link.click()
      
      this.$http({
          url: this.$http.adornUrl(`/ofs/table/option/gen/excel`),
          method: 'post',
          data: this.numberValidateForm.time,
          responseType: 'blob',
          }).then( res => {
            this.download(res.data , `${this.numberValidateForm.time}货物数据.xlsx`)
        })

      },
      submitForm2(){
        let link = document.createElement('a')
        link.setAttribute('href', '../../../../../static/sheettable.xlsx')
        
        link.setAttribute('download', '2020年9月货物数据.xlsx')
        link.click()
      },
      // resetForm(formName) {
      //   this.$refs[formName].resetFields();
      // }
      download (data,titName) {
        if(!data){
          return
        }
        const content = data
        const blob = new Blob([content],{type: "application/vnd.ms-excel"})
        const fileName = titName?titName: '数据.xlsx'
        if ('download' in document.createElement('a')) { // 非IE下载
          const elink = document.createElement('a')
          elink.download = fileName
          elink.style.display = 'none'
          elink.href = URL.createObjectURL(blob)
          document.body.appendChild(elink)
          elink.click()
          URL.revokeObjectURL(elink.href) // 释放URL 对象
          document.body.removeChild(elink)
        } else { // IE10+下载
          navigator.msSaveBlob(blob, fileName)
        }
      }
    }
  }
</script>