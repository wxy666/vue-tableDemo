<template>
  <div v-if="data" class="tk">
    <el-form :model="data"
             :rules="rules"
             ref="hh">
      <el-form-item label="人员"
                    prop="user">
        <el-input v-model="data.user"></el-input>
      </el-form-item>
      <el-form-item label="标题"
                    prop="content">
        <el-input type="textarea"
                  v-model="data.content"></el-input>
      </el-form-item>
      <el-form-item label="日期"
                    prop="summary">
        <el-input type="textarea"
                  v-model="data.summary"></el-input>
      </el-form-item>
      <el-form-item>
        <el-button type="primary"
                   @click="edita">确定</el-button>
        <el-button type="cancel"
                   @click="$emit('cancel-dialog')">取消</el-button>
      </el-form-item>
    </el-form>
  </div>
</template>
<script>
export default {
  props: ['data'], //父组件传参给子组件
  data () {
    return {    
      rules: {
        content: { required: true, message: '请输入标题', trigger: 'blur' },
        user: { required: true, message: '请输入内容', trigger: 'blur' }
      }
    }
  },
  mounted () {
		this.init()
  },
  computed: {
    modifylist(){
    	return this.data;
    }
  },
  methods: {
    edita: function () {
      this.$refs.hh.validate(valid => {
        if (valid) {
          console.log(this.data)
          this.$emit('edita', this.modifylist)
        }
      })
    },
    init:function(){
    	console.log(this.data)
    }
  }
}
</script>