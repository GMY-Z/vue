<template>
  <el-container>
    <el-breadcrumb separator="/">
      <el-breadcrumb-item><i class="el-icon-document"></i>存款记录</el-breadcrumb-item>
    </el-breadcrumb>
      <el-header style="text-align: right; font-size: 15px; height: 160px">
        <div class="block">
          <span class="demonstration">操作日期：</span>
          <el-date-picker
            v-model="value"
            align="right"
            type="date"
            value-format="yyyy-MM-dd"
            placeholder="选择日期"
            :picker-options="pickerOptions">
          </el-date-picker>
        </div>
        <el-button type="primary" icon="el-icon-search" @click="search_bydate(this.value)">搜索</el-button>
      </el-header>
      <el-main>
        <el-table :data="tableData" max-height="250">
          <el-table-column
            :key="col.prop"
            :label="col.label"
            :prop="col.prop"
            v-for="col in cols">
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
        tableData: [],
        cols: [],
        pickerOptions: {
          disabledDate(time) {
            return time.getTime() > Date.now();
          },
          shortcuts: [{
            text: '今天',
            onClick(picker) {
              picker.$emit('pick', new Date());
            }
          }, {
            text: '昨天',
            onClick(picker) {
              const date = new Date();
              date.setTime(date.getTime() - 3600 * 1000 * 24);
              picker.$emit('pick', date);
            }
          }, {
            text: '一周前',
            onClick(picker) {
              const date = new Date();
              date.setTime(date.getTime() - 3600 * 1000 * 24 * 7);
              picker.$emit('pick', date);
            }
          }]
        },
        value: ''
      }
    },
    methods: {
      search () {
        const self = this;	
				let username = localStorage.getItem('ms_user').name;
        this.$http.post('/api/user/Csearch_deposit',{name: username}).then(res => {
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
      search_bydate(value){
        const self = this;	
				let username = localStorage.getItem('ms_user').name;
        this.$http.post('/api/user/Csearch_deposit_bytime',{name: username, time: value}).then(res => {
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
      }
    },
    mounted() {
      this.search();
    },
};
</script>