<template>
  <el-container>
    <el-breadcrumb separator="/">
      <el-breadcrumb-item><i class="el-icon-document"></i>存款记录</el-breadcrumb-item>
    </el-breadcrumb>
      <el-header style="text-align: right; font-size: 15px; height: 160px">
        <div class="block">
          <span class="demonstration">存款类型：</span>
          <el-cascader 
          ref="cascaderAddr" 
          v-model="areaCode" 
          :options="options" 
          @change="change()"
          clearable> 
          </el-cascader>
        </div>
        <div class="block">
          <span class="demonstration">操作日期：</span>
          <el-date-picker
            v-model="value"
            align="right"
            type="date"
            placeholder="选择日期"
            :picker-options="pickerOptions">
          </el-date-picker>
        </div>
        <el-button type="primary" icon="el-icon-search" @click="search_by(this.value, this.areaCode)">搜索</el-button>
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
        options: [{
          value: '活期',
          label: '活期'
        }, 
        { value: '整存整取',
          label: '整存整取',
          children:[{
          value: '3个月',
          label: '3个月'
        }, {
          value: '6个月',
          label: '6个月'
        }, {
          value: '1年',
          label: '1年'
        }, {
          value: '3年',
          label: '3年'
        }, {
          value: '5年',
          label: '5年'
        }]},{
          value: '零存整取',
          label: '零存整取',
          children:[{
          value: '3个月',
          label: '3个月'
        }, {
          value: '6个月',
          label: '6个月'
        }, {
          value: '1年',
          label: '1年'
        }, {
          value: '3年',
          label: '3年'
        }, {
          value: '5年',
          label: '5年'
        }]},{
          value: '整存零取',
          label: '整存零取',
          children:[{
          value: '3个月',
          label: '3个月',
          children:[{
          value: '1个月',
          label: '1个月'
        },{
          value: '3个月',
          label: '3个月'
        }, {
          value: '6个月',
          label: '6个月'
        }]
        }, {
          value: '6个月',
          label: '6个月',
        children:[{
          value: '1个月',
          label: '1个月'
        },{
          value: '3个月',
          label: '3个月'
        }, {
          value: '6个月',
          label: '6个月'
        }]
        },  {
          value: '1.95%',
          label: '1年',
        children:[{
          value: '1个月',
          label: '1个月'
        },{
          value: '3个月',
          label: '3个月'
        }, {
          value: '6个月',
          label: '6个月'
        }]
        }, {
          value: '3年',
          label: '3年',
        children:[{
          value: '1个月',
          label: '1个月'
        },{
          value: '3个月',
          label: '3个月'
        }, {
          value: '6个月',
          label: '6个月'
        }]
        },  {
          value: '5年',
          label: '5年',
        children:[{
          value: '1个月',
          label: '1个月'
        },{
          value: '3个月',
          label: '3个月'
        }, {
          value: '6个月',
          label: '6个月'
        }]
        }, ]
        }],
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
      change(){
        this.check=true;
        console.log(this.form.areaCode);
        if(this.$refs.cascaderAddr.getCheckedNodes()[0].pathLabels[0]=="整存零取"||
        this.$refs.cascaderAddr.getCheckedNodes()[0].pathLabels[0]=="零存整取")
        {
          this.check=false;
        }
      },
      search () {
        const self = this;	
        this.$http.post('/api/user/Asearch_deposit').then(res => {
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
      search_by(value, areaCode){
        const self = this;	
        this.$http.post('/api/user/Asearch_deposit_by', {time: value, deposit_kind: areaCode}).then(res => {
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