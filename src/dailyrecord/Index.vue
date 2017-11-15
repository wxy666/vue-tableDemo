<template>
  <div id="main">
    <el-button @click="addlog = true">新增</el-button>
    <!-- 新增 -->
    <el-dialog title="新增日志"
               :visible.sync="addlog"
               v-model="xlist">
      <add-log @cancel-dialog="addlog=false" @submit-btn="addlogl" ref="form"></add-log>
    </el-dialog>
    <!-- 修改 -->
   <el-dialog title="修改日志"
               :visible.sync="editlog"
               v-model="editlog"
                >
        <edit-log @cancel-dialog="editlog=false"
               @edita="edita" :data="editData"  ref="form"></edit-log>
    </el-dialog>
    
    <el-table :data="xlist"
              width="100%"
              max-height="500"
              stripe
              border
              ref="multipleTable">
      <el-table-column type="selection"
                       width="55">
      </el-table-column>
      <el-table-column label="日期"
                       width="350"
                       prop="summary">
      </el-table-column>
      <el-table-column label="人员"
                       width="250"
                       prop="user"></el-table-column>
      <el-table-column label="标题"
                       prop="content"
                       width="840">
      </el-table-column>
      <el-table-column label="操作"
                       width="405">
        <template slot-scope="scope">
          <el-button size="mini"
                     @click="edit(scope.$index, scope.row)">编辑</el-button>
          <el-button size="mini"
                     type="danger"
                     @click.native.prevent="deleteRow(scope.$index, scope.row)">删除</el-button>
        </template>
      </el-table-column>
    </el-table>
  </div>
</template>
<script>
import AddLog from '@/dailyrecord/AddLog'
import EditLog from '@/dailyrecord/EditLog'
export default {
  data () {
    return {
      xlist: [],
      addlog: false,
      editlog: false,
      editmodel: false,
      editData: {
        content: '',
        summary: '',
        user: '',
        id: ''
      }
    }
  },
  methods: {
    loadlist () {
      this.$axios.get('http://qianjia.space:8000/logs')
        .then(res => {
          this.xlist = res.data
        })
    },
    addlogl (formdata) {
      this.$axios.post('http://qianjia.space:8000/logs', formdata)
        .then((res) => {
          this.addlog = false
          this.loadlist();
          alert(2)
        })
    },
    deleteRow (index, row) {
      var id = row._id.$oid
      this.$axios.delete('http://qianjia.space:8000/logs' + '/' + id)
        .then((res) => {
          this.xlist.splice(index, 1)
          this.loadlist()
        })
      this.$message('删除成功')
    },
    edit (index,row) {
    	alert(JSON.stringify(row))
      this.editData = row	 //  赋值
      this.editlog = true
     
    },
    edita (newObj) {	//点击保存按钮
      var newObj=JSON.parse(JSON.stringify(newObj))
      console.log('我获取的是传过来的值', newObj)
     
      let json = {
        'summary': this.editData.summary,
        'content': this.editData.content,
        'user': this.editData.user
      }
      this.$axios.put('http://qianjia.space:8000/logs' + '/' + newObj._id.$oid, json)
      .then((res) => {
        this.editlog = false
      })
    },
  },
  // 初始化
  mounted () {
    this.loadlist()
    // this.formdata = this.xlist
  },
  components: {
    AddLog,
    EditLog
  }
}
</script>
<style>
</style>