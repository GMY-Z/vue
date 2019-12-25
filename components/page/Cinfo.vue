<template>
  <div>
    <div class="crumbs">
      <el-breadcrumb separator="/">
        <el-breadcrumb-item>
          <i class="el-icon-user"></i>个人中心
        </el-breadcrumb-item>
      </el-breadcrumb>
    </div>
    <div class="userContent">
      <el-form ref="form" :model="form" label-width="80px">
        <el-form-item label="卡号">
          <el-input v-model="form.card_id" :disabled="true"></el-input>
        </el-form-item>
        <el-form-item label="姓名">
          <el-input v-model="form.fullname" :disabled="true"></el-input>
        </el-form-item>
        <el-form-item label="性别">
          <el-select class="select-sex" v-model="form.gender" :disabled="true">
            <el-option label="男" value="man"></el-option>
            <el-option label="女" value="woman"></el-option>
          </el-select>
        </el-form-item>
        <el-form-item label="身份证号">
          <el-input v-model="form.resident_id" :disabled="true"></el-input>
        </el-form-item>
				<el-form-item label="账户总额">
          <el-input v-model="form.account" :disabled="true"></el-input>
				</el-form-item>
        <el-form-item label="联系方式">
          <el-input v-model="form.phone"></el-input>
        </el-form-item>
        <el-form-item label="住址">
          <el-input v-model="form.address"></el-input>
        </el-form-item>
        <div class="revise-btn">
          <el-button type="primary" @click="submitForm('form')">修改</el-button>
        </div>
      </el-form>
    </div>
  </div>
</template>

<script>
	export default {
		data() {
            return {
				form: {
					card_id:'',
					fullname:'',
					gender:'',
					resident_id:'',
					account:'',
					phone:'',
					address:''
				}
			}
		},
		methods:{
			getUserData() {
				const self = this;	
				let username = localStorage.getItem('ms_user').name;			
				self.$http.get('/api/user/getClient',{name: username}).then(function(response) {
					console.log(response);
					let result = response.data[0];
					self.form.card_id = result.card_id;
					self.form.fullname = result.fullname;
					self.form.gender = result.gender;
					self.form.resident_id = result.resident_id;
					self.form.account = result.account;
					self.form.phone = result.phone;
					self.form.address = result.address;
				}).then(function(error) {
					console.log(error);
				})
			},
			submitForm(formName) {
				this.$confirm('确认修改个人信息?', '提示', {
          confirmButtonText: '确定',
          cancelButtonText: '取消',
          type: 'warning'
        }).then(() => {
					const self = this;
				self.$http.post('/api/user/changeClient',self.form)
				.then((response) => {
					console.log(response);
					if (response.status == 200) {
						let result = response.data[0];
						self.form.phone = result.phone;
						self.form.address = result.address;
						this.$message({
            type: 'success',
            message: '修改成功!'
          	});  
					}
					else{
						this.$message({
            type: 'info',
            message: '修改失败'
          })
					}                            
				}).then((error) => {
					this.$message({
            type: 'info',
            message: '发送请求失败'
          });     
					console.log(error);
				})})
		}},
		mounted() {
			this.getUserData();
		}    	
}
	
</script>

<style>
.userContent {
  width: 400px;
  margin: 0 auto;
}
.revise-btn {
  width: 400px;
  text-align: center;
}
.select-sex {
  width: 320px;
}
</style>