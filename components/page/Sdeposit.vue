<template>
	<div>
		<div class="drawer">
			<el-drawer
				title="新增存款记录"
				:visible.sync="drawer"
				direction="rtl"
				size="70%">
				<el-table :data="tableData" max-height="250">
          <el-table-column
            :key="col.prop"
            :label="col.label"
            :prop="col.prop"
            v-for="col in cols">
          </el-table-column>
        </el-table>
			</el-drawer>
		</div>
		<div class="crumbs">
			<el-breadcrumb separator="/">
        <el-breadcrumb-item><i class="el-icon-document-add"></i>存款办理</el-breadcrumb-item>
      </el-breadcrumb>
		</div>
		<div class="userContent">
			<el-form ref="form" :model="form" label-width="80px">
        <el-form-item label="储户卡号">
          <el-input
            style="width:220px"
            v-model="form.card_id">
          </el-input>
        </el-form-item>
				<el-form-item label="存款金额">
					<el-input 
          style="width:220px"
          v-model="form.deposit_amount"></el-input>
				</el-form-item>
				<el-form-item label="起息日">
					<el-date-picker
            v-model="form.value_date"
            align="right"
            type="date"
            value-format="yyyy-MM-dd"
            placeholder="选择日期"
            :picker-options="pickerOptions">
          </el-date-picker>
				</el-form-item>
        <el-form-item label="存款类型">
          <el-cascader 
          ref="cascaderAddr" 
          v-model="form.areaCode" 
          :options="options" 
          @change="change()"
          clearable> 
          </el-cascader>
				</el-form-item>
        <el-form-item label="零存取金额" label-width="100px">
					<el-input 
          style="width:200px"
          :disabled="check"
          v-model="form.fixed_amount"></el-input>
				</el-form-item>
				<div class="revise-btn">
          <el-button type="primary" @click="submitForm('form')">办理</el-button>
        </div>
			</el-form>
		</div>
	</div>
</template>

<script>
 export default {
		 data() {
            return {
        drawer: false,
        check:true,
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
          children:[ {
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
          value: '1年',
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
            return time.getTime() < Date.now();
          },
          shortcuts: [{
            text: '今天',
            onClick(picker) {
              picker.$emit('pick', new Date());
            }
          }, {
            text: '明天',
            onClick(picker) {
              const date = new Date();
              date.setTime(date.getTime() + 3600 * 1000 * 24);
              picker.$emit('pick', date);
            }
          }, {
            text: '一周后',
            onClick(picker) {
              const date = new Date();
              date.setTime(date.getTime() + 3600 * 1000 * 24 * 7);
              picker.$emit('pick', date);
            }
          }]
				},
				form: {
          card_id:'',
          deposit_amount:'',
          value_date:'',
          areaCode:'',
          fixed_amount:''
				}
			}
  },
	methods: {
    submitForm(formName) {
      this.$confirm('确认办理此存款业务?', '提示', {
        confirmButtonText: '确定',
        cancelButtonText: '取消',
        type: 'warning'
      }).then(() => {
        const self = this;
      let username = localStorage.getItem('ms_user').name;
      self.form.push({number:username});
      self.form.areaCode=self.choice;
      console.log(self.form);
      self.$http.post('/api/user/Sdeposit',self.form)
      .then((res) => {
        console.log(res);
        if (res.status == 200) {
          this.drawer=true;
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
          this.$message({
          type: 'success',
          message: '办理成功!'
          });  
        }
        else{
          this.$message({
          type: 'info',
          message: '办理失败'
        })
        }                            
      }).then((error) => {
        this.$message({
          type: 'info',
          message: '发送请求失败'
        });     
        console.log(error);
      })})
  },
  change(){
    this.check=true;
    console.log(this.form.areaCode);
   if(this.$refs.cascaderAddr.getCheckedNodes()[0].pathLabels[0]=="整存零取"||
        this.$refs.cascaderAddr.getCheckedNodes()[0].pathLabels[0]=="零存整取")
    {
      this.check=false;
    }
  }
  },
};
</script>

<style>
	.userContent {
		width: 400px;
		margin: 0 auto;
	}
	.revise-btn{
      width: 400px;
			text-align: center;
  }
	.select-sex {
		width: 320px;
	}
</style>