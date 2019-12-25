<template>
	<div>
		<div class="drawer">
			<el-drawer>
				title="利息清单"
				:visible.sync="drawer"
				direction="rtl"
				size="70%">
				<el-table :data="newtableData" max-height="250">
          <el-table-column
            :key="col.prop"
            :label="col.label"
            :prop="col.prop"
            v-for="col in cols">
          </el-table-column>
        </el-table>
			</el-drawer>
		</div>
		<el-container>
    <el-breadcrumb separator="/">
      <el-breadcrumb-item><i class="el-icon-coin"></i>取款办理</el-breadcrumb-item>
    </el-breadcrumb>
      <el-header style="text-align: right; font-size: 15px; height: 160px">
        <div class="block">
          <span class="demonstration">储户卡号：</span> 
          <el-input
            size="small"
            style="width:220px"
            v-model="card_id">
          </el-input>
        </div>
        <el-button type="primary" icon="el-icon-search" @click="search_bycard_id(this.card_id)">搜索</el-button>
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
          <template slot-scope="scope">
              <el-button
                @click.native.prevent="withdraw(scope.row.serial_number)"
                type="primary"
                size="small">
                取款
              </el-button>
            </template>
          </el-table-column>
        </el-table>
      </el-main>
    </el-container>
		 <el-dialog title="取款金额" :visible.sync="outerVisible" >
    <el-slider v-model="value" :max="max_value" show-input></el-slider>
    <div slot="footer" class="dialog-footer">
      <el-button @click="outerVisible = false">取 消</el-button>
      <el-button type="primary" @click="submit()">确 定</el-button>
    </div>
  </el-dialog>
	</div>
</template>

<script>
	export default {
		data() {
      return {
				outerVisible: false,
        innerVisible: false,
        drawer:false,
        value:0,
        serial:0,
        max_value:100000,
				form:{}
			}
		},
		methods:{
        withdraw(serial_number){
          this.serial=serial_number;
          this.outerVisible=true;
        },
        submit() {
          this.outerVisible=false;
          this.$confirm('确认办理取款业务?', '提示', {
          confirmButtonText: '确定',
          cancelButtonText: '取消',
          type: 'warning'
        }).then(() => {
        let username = localStorage.getItem('ms_user').name;
        this.$http.post('/api/user/Swithdrawal',{serial_number:this.serial,withdrawal_amount:this.value,
        name:username}).then(res => {
          this.cols = []
          for (var j = 0; j < res.data.data.cols_list.length; j++) {
            var obj = {}
            obj.label = res.data.data.cols_list[j].label
            obj.prop = res.data.data.cols_list[j].prop
            this.cols.push(obj)
          }
          console.log(this.cols)
          this.newtableData = []
          for (var i = 0; i < res.data.data.each_row.length; i++) {
            this.newtableData.push(res.data.data.each_row[i])
          }
          console.log(this.newtableData)
        }, (error) => {
          this.$message.error('查询失败' + error.response.data.msg)
        })
        this.$message({
        type: 'success',
        message: '办理成功!',
      })
        this.drawer=true;
      }).catch(() => {
        this.$message({
          type: 'info',
          message: '已取消操作'
        });          
      });
    },
    search () {
        const self = this;	
        this.$http.post('/api/user/Swithdrawal_search').then(res => {
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
    search_bycard_id(card_id){
        this.drawer=true;
        const self = this;	
        this.$http.post('/api/user/Swithdrawal_search_bycard_id',{name: card_id}).then(res => {
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