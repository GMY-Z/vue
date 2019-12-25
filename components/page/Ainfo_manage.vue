<template>
  <el-container>
    <el-breadcrumb separator="/">
      <el-breadcrumb-item><i class="el-icon-info"></i>业务员信息管理</el-breadcrumb-item>
    </el-breadcrumb>
      <el-header style="text-align: right; font-size: 15px; height: 160px">
        <div class="block">
          <span class="demonstration">业务员编号：</span> 
          <el-input
            size="small"
            style="width:220px"
            v-model="card_id">
          </el-input>
        </div>
        <el-button type="primary" icon="el-icon-search">搜索</el-button>
      </el-header>
      <el-main>
          <el-table :data="tableData" max-height="250">
          <el-table-column
            :key="col.prop"
            :label="col.label"
            :prop="col.prop"
            v-for="col in cols">
          </el-table-column>
          <el-table-column>
           <template slot-scope="{row,$index}">
            <el-button type="text" size="small"     @click.native="handleCancel($index, row)"     v-if="showBtn[$index]">取消</el-button>
            <el-button type="text" size="small"     @click.native="handleEdit($index, row)"     v-if="!showBtn[$index]">编辑</el-button>
            <el-button type="text" size="small"     @click.native="handleDelete($index, row)"     v-if="!showBtn[$index]">删除</el-button>
          </template>
          </el-table-column>
        </el-table>
      </el-main>
    </el-container>
</template>

<style>
.el-header {
    background-color: rgb(255, 255, 255);
    color: rgb(0, 0, 0);
    line-height: 60px;
  }
  
  .el-aside {
    color: rgb(255, 255, 255);
  }
</style>

<script>
  export default {
    data() {
      return {
        showEdit: [], //显示编辑框
        showBtn: [],
        showBtnOrdinary: true
      }
      for(var i = 0; i < 100; i ++) {
      this.showEdit[i] = false;
      this.showBtn[i] = false;
      this.$set(vm.showEdit[i], false);
      this.$set(vm.showBtn[i], false);
      }
    },
    methods:{
      search () {
        const self = this;	
        this.$http.post('/api/user/Ainfo_initialize').then(res => {
          this.cols = []
          for (var j = 0; j < res.data.data.cols_list.length; j++) {
            var obj = {}
            obj.label = res.data.data.cols_list[j].label
            obj.prop = res.data.data.cols_list[j].prop
            this.cols.push(obj)
          }
          console.log(this.cols)
          this.tableData = []
          for (var i = 0; i < res.data.data.each_row.length; i++) {
            this.tableData.push(res.data.data.each_row[i])
          }
          console.log(this.tableData)
        }, (error) => {
          this.$message.error('查询失败' + error.response.data.msg)
        })
      },
            //点击编辑
      handleEdit(index, row) {
          this.showEdit[index] = true;
          this.showBtn[index] = true;
          this.$set(this.showEdit,index,true)
          this.$set(this.showBtn,index,true)
          this.$http.post('/api/user/Ainfo_edit', {number:row.number, fullname:row.fullname, 
          gender:row.gender, phone:row.phone, address:row.address, password:row.password}).then(res => {
          this.cols = []
          for (var j = 0; j < res.data.data.cols_list.length; j++) {
            var obj = {}
            obj.label = res.data.data.cols_list[j].label
            obj.prop = res.data.data.cols_list[j].prop
            this.cols.push(obj)
          }
          console.log(this.cols)
          this.tableData = []
          for (var i = 0; i < res.data.data.each_row.length; i++) {
            this.tableData.push(res.data.data.each_row[i])
          }
          console.log(this.tableData)
        }, (error) => {
          this.$message.error('修改失败' + error.response.data.msg)
        })
      },
      //取消编辑
      handelCancel(index, row) {
          this.getContentList();
          this.showEdit[index] = false;
          this.showBtn[index] = false;
                  },
      //点击删除
      handleDelete(index, row) {
        this.$http.post('/api/user/Ainfo_delete', {number:row.number}).then(res => {
          this.cols = []
          for (var j = 0; j < res.data.data.cols_list.length; j++) {
            var obj = {}
            obj.label = res.data.data.cols_list[j].label
            obj.prop = res.data.data.cols_list[j].prop
            this.cols.push(obj)
          }
          console.log(this.cols)
          this.tableData = []
          for (var i = 0; i < res.data.data.each_row.length; i++) {
            this.tableData.push(res.data.data.each_row[i])
          }
          console.log(this.tableData)
        }, (error) => {
          this.$message.error('删除失败' + error.response.data.msg)
        })
      },
    },
    mounted() {
      this.search();
    },
  };
</script>