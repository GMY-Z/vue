<template>
  <el-container>
    <el-breadcrumb separator="/">
      <el-breadcrumb-item><i class="el-icon-bank-card"></i>银行业务总览</el-breadcrumb-item>
    </el-breadcrumb>
      <el-header style="text-align: center; font-size: 15px; height: 260px;margin-top:40px">
        <el-tabs type="border-card">
  <el-tab-pane>
    <span slot="label"><i class="el-icon-office-building"></i>银行当前资产</span>
    <el-form ref="form" :model="form" label-width="120px">
      <el-form-item label="存款总额：">
					<el-input v-model="form.deposit" :disabled=true></el-input>
			</el-form-item>
			<el-form-item label="活期存款金额：">
				<el-input v-model="form.current_deposit" :disabled=true></el-input>
			</el-form-item>
			<el-form-item label="整存整取金额：">
				<el-input v-model="form.whole_whole" :disabled=true></el-input>				
      </el-form-item>
      <el-form-item label="整存活取金额：">
				<el-input v-model="form.whole_part" :disabled=true></el-input>
			</el-form-item>
			<el-form-item label="活存整取金额：">
				<el-input v-model="form.part_whole" :disabled=true></el-input>
			</el-form-item>
    </el-form>
  </el-tab-pane>
  <el-tab-pane label="本年交易">
    <div class="block">
  <div class="radio">
    排序：
    <el-radio-group v-model="reverse">
      <el-radio :label="true">倒序</el-radio>
      <el-radio :label="false">正序</el-radio>
    </el-radio-group>
  </div>

  <el-timeline :reverse="reverse">
    <el-timeline-item
      v-for="(activity, index) in activities"
      :key="index"
      :icon="activity.icon"
      :type="activity.type"
      :color="activity.color"
      :timestamp="activity.timestamp">
      {{activity.content}}
    </el-timeline-item>
  </el-timeline>
</div>
  </el-tab-pane>
  <el-tab-pane label="本月交易">
    <div class="block">
  <div class="radio">
    排序：
    <el-radio-group v-model="reverse">
      <el-radio :label="true">倒序</el-radio>
      <el-radio :label="false">正序</el-radio>
    </el-radio-group>
  </div>

  <el-timeline :reverse="reverse">
    <el-timeline-item
      v-for="(activity, index) in activities"
      :key="index"
      :icon="activity.icon"
      :type="activity.type"
      :color="activity.color"
      :timestamp="activity.timestamp">
      {{activity.content}}
    </el-timeline-item>
  </el-timeline>
</div>
  </el-tab-pane>
  <el-tab-pane label="本日交易">
    <div class="block">
  <div class="radio">
    排序：
    <el-radio-group v-model="reverse">
      <el-radio :label="true">倒序</el-radio>
      <el-radio :label="false">正序</el-radio>
    </el-radio-group>
  </div>

  <el-timeline :reverse="reverse">
    <el-timeline-item
      v-for="(activity, index) in activities"
      :key="index"
      :icon="activity.icon"
      :type="activity.type"
      :color="activity.color"
      :timestamp="activity.timestamp">
      {{activity.content}}
    </el-timeline-item>
  </el-timeline>
</div>
  </el-tab-pane>
</el-tabs>
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
        reverse: true,
        form:{},
        activities: [{
          content: '存入金额xxxx',
          timestamp: '2018-04-15',
          size: 'large',
          type: 'primary',
          icon:'el-icon-more'
        }, {
          content: '取出金额xxxx',
          timestamp: '2018-04-13',
          size: 'large',
          type: 'primary',
          icon:'el-icon-more'
        }]
    }},
    methods: {
      search () {
        const self = this;	
        self.$http.get('/api/user/Adeposit').then(function(response) {
					console.log(response);
					let result = response.data[0];
					self.form.deposit = result.deposit;
					self.form.current_deposit = result.current_deposit;
					self.form.whole_whole = result.whole_whole;
					self.form.whole_part = result.whole_part;
					self.form.part_whole = result.part_whole;
				}).then(function(error) {
					console.log(error);
				})
      }
    },
    mounted() {
      this.search();
    },
  };
</script>