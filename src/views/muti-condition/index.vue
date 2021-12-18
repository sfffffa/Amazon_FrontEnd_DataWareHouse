<template>
  <div class="app-container">
    <el-form ref="form" :model="form" label-width="120px">
      <el-form-item label="时间">
        <el-row :gutter="10">
          <el-col :span="6">
            <div class="grid-content">
              <el-select v-model="formMark.time" placeholder="">
                <el-option label="年" value="year" />
                <el-option label="年/月" value="month" />
                <el-option label="年/季度" value="quarter" />
              </el-select>
            </div>
          </el-col>
          <el-col :span="6">
            <div class="grid-content">
              <el-input v-model="form.time.year" placeholder="请输入年份" />
            </div>
          </el-col>
          <el-col :span="6" v-if="formMonthMark">
            <div class="grid-content">
              <el-input v-model="form.time.month" placeholder="请输入月份" />
            </div>
          </el-col>
          <el-col :span="6" v-if="formQuarterMark">
            <div class="grid-content">
              <el-input v-model="form.time.quarter" placeholder="请输入季度" />
            </div>
          </el-col>
        </el-row>     
      </el-form-item>

      <el-form-item label="电影名">
        <el-row>
          <el-col :span="18"><div class="grid-content">
            <el-input v-model="form.name" placeholder="请输入电影名"/>
          </div></el-col>
        </el-row>        
      </el-form-item>

      <el-form-item label="导演">
        <el-row>
          <el-col :span="18"><div class="grid-content">
            <el-input v-model="form.director" placeholder="请输入导演名"/>
          </div></el-col>
        </el-row>        
      </el-form-item>

      <el-form-item label="演员">
        <el-row :gutter="10">
          <el-col :span="6">
            <div class="grid-content">
              <el-select v-model="formMark.actor" placeholder="">
                <el-option label="演员参演" value="actor" />
                <el-option label="演员主演" value="mainActor" />
              </el-select>
            </div>
          </el-col>
          <el-col :span="12" v-if="formActorMark">
            <div class="grid-content">
              <el-input v-model="form.actor.actor" placeholder="请输入演员名称" />
            </div>
          </el-col>
          <el-col :span="12" v-if="formMainActorMark">
            <div class="grid-content">
              <el-input v-model="form.actor.mainActor" placeholder="请输入演员名称" />
            </div>
          </el-col>
        </el-row>     
      </el-form-item>

      <el-form-item label="Activity zone">
        <el-select v-model="form.region" placeholder="please select your zone">
          <el-option label="Zone one" value="shanghai" />
          <el-option label="Zone two" value="beijing" />
        </el-select>
      </el-form-item>

      <el-form-item label="Activity time">
        <el-col :span="11">
          <el-date-picker v-model="form.date1" type="date" placeholder="Pick a date" style="width: 100%;" />
        </el-col>
        <el-col :span="2" class="line">-</el-col>
        <el-col :span="11">
          <el-time-picker v-model="form.date2" type="fixed-time" placeholder="Pick a time" style="width: 100%;" />
        </el-col>
      </el-form-item>
      <el-form-item label="Instant delivery">
        <el-switch v-model="form.delivery" />
      </el-form-item>
      <el-form-item label="Activity type">
        <el-checkbox-group v-model="form.type">
          <el-checkbox label="Online activities" name="type" />
          <el-checkbox label="Promotion activities" name="type" />
          <el-checkbox label="Offline activities" name="type" />
          <el-checkbox label="Simple brand exposure" name="type" />
        </el-checkbox-group>
      </el-form-item>
      <el-form-item label="Resources">
        <el-radio-group v-model="form.resource">
          <el-radio label="Sponsor" />
          <el-radio label="Venue" />
        </el-radio-group>
      </el-form-item>
      <el-form-item label="Activity form">
        <el-input v-model="form.desc" type="textarea" />
      </el-form-item>
      <el-form-item>
        <el-button type="primary" @click="onSubmit">Create</el-button>
        <el-button @click="onCancel">Cancel</el-button>
      </el-form-item>
    </el-form>
  </div>
</template>

<script>
export default {
  data() {
    return {
      form: {
        time: {
          year: '',
          month: '',
          quarter: '',
        },
        name: '',
        director: '',
        actor:{
          actor: '',
          mainActor: '',
        },
        region: '',
        date1: '',
        date2: '',
        delivery: false,
        type: [],
        resource: '',
        desc: ''
      },
      formMark:{
        time: 'year',
        actor: 'actor',
      }
    }
  },
  computed:{
    formMonthMark(){
      return this.formMark.time=="month";
    },
    formQuarterMark(){
      return this.formMark.time=="quarter";
    },
    formActorMark(){
      return this.formMark.actor=="actor";
    },
    formMainActorMark(){
      return this.formMark.actor=="mainActor";
    },
  },
  methods: {
    onSubmit() {
      this.$message('submit!')
    },
    onCancel() {
      this.$message({
        message: 'cancel!',
        type: 'warning'
      })
    }
  }
}
</script>

<style scoped>
  .line{
    text-align: center;
  }
  .el-row {
    margin-bottom: 20px;
  }
  .el-col {
    border-radius: 4px;
  }
  .bg-purple-dark {
    background: #99a9bf;
  }
  .bg-purple {
    background: #d3dce6;
  }
  .bg-purple-light {
    background: #e5e9f2;
  }
  .grid-content {
    border-radius: 4px;
    min-height: 36px;
  }
  .row-bg {
    padding: 10px 0;
    background-color: #f9fafc;
  }
</style>
