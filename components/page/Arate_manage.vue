<template>
  <el-container>
    <el-breadcrumb separator="/">
      <el-breadcrumb-item><i class="el-icon-money"></i>利率管理</el-breadcrumb-item>
    </el-breadcrumb>
      <el-header margin-top="40px" style="text-align: right; font-size: 15px; height: 160px">
        <el-button type="primary" @click="addRow(tableData)">新增</el-button>
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
      </el-header>
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
        showBtnOrdinary: true,
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
        }
        for(var i = 0; i < 100; i ++) {
          this.showEdit[i] = false;
          this.showBtn[i] = false;
          this.$set(vm.showEdit[i], false);
          this.$set(vm.showBtn[i], false);
      }
    },
    methods:{
      handleEdit(index, row) {
                this.showEdit[index] = true;
                this.showBtn[index] = true;
                this.$set(this.showEdit,index,true)
                this.$set(this.showBtn,index,true)
                this.$http.post('/api/user/Arate_edit', {deposit_id:row.deposit_id, deposit_name:row.deposit_name, 
                deposit_period:row.deposit_period}).then(res => {
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
                this.$set(this.showEdit,index,false)
                this.$set(this.showBtn,index,false)
            },
      addRow(tableData,event){
                tableData.push({ deposit_id: '', deposit_name: '',deposit_period:'',interest_rate:'' })
            },
      search () {
        const self = this;	
        this.$http.post('/api/user/Arate').then(res => {
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
          }
  }
</script>