<template>
  <el-form :model="formData"
           :rules="rules"
           ref="forma">
    <el-form-item label="人员"
                  prop="user">
      <el-input v-model="formData.user"></el-input>
    </el-form-item>
    <el-form-item label="标题"
                  prop="content">
      <el-input type="textarea"
                v-model="formData.content"></el-input>
    </el-form-item>
    <el-form-item label="日期"
                  prop="summary">
      <el-input type="textarea"
                v-model="formData.summary"></el-input>
    </el-form-item>
    <el-form-item>
      <el-button type="primary"
                 @click="addlogone">确定</el-button>
      <el-button type="cancel"
                 @click="$emit('cancel-dialog')">取消</el-button>
    </el-form-item>
  </el-form>
</template>
<script>
export default {
  data () {
    return {
    	formData:{},
      rules: {
        content: { required: true, message: '请输入标题', trigger: 'blur' },
        user: { required: true, message: '请输入内容', trigger: 'blur' }
      }
    }
  },
  mounted(){
  		this.init()
  },
  computed:{
  	
  },
  methods: {
  	init:function(){
  		this.formData={
  			user:'',
  			content:'',
  			summary:''
  		}
  	},
    addlogone: function () {  
      this.$refs.forma.validate((val) => {
        if (val) {    
        	//（子组件不能直接父级的数据，需要用data或者computed生成一个局部变量，然后再使用$emit方法把这个局部数据再传递上去）
          this.$emit('submit-btn', this.formData)
        }
      })
    }
  } 
}
</script>