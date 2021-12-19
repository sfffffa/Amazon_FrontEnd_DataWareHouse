<template>
  <div class="app-container">
    <el-card style="width:100%; margin:0 auto; padding-top: 20px">
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
            <el-col :span="2" :push="1" >
              <div class="grid-content">
                <el-button type="primary" @click="onSubmit">查询</el-button>
              </div>
            </el-col>
          </el-row>     
        </el-form-item>
      </el-form>
    </el-card>
    <el-card style="width:100%; margin:2% auto; height: 410px;">
      <el-row>
        <el-col :span="15"><div class="grid-content">
          <el-table
            :data="tableData"
            :stripe="true"
            height="330"
            :highlight-current-row='true'
            style="width: 95%; margin: 0; height: 330px;">
            <el-table-column
              prop="movie_id"
              label="电影ID"
              align='center'
              width="90">
            </el-table-column>
            <el-table-column
              prop="title"
              label="电影名称"
              align='center'
              width="300">
            </el-table-column>
            <el-table-column
              prop="runtime"
              label="电影时长"
              align='center'
              width="180">
            </el-table-column>
            <el-table-column
              prop="releasedate"
              label="发行日期"
              align='center'>
            </el-table-column>
          </el-table>
          <el-pagination
            layout="total"
            style="margin: 0 auto;"
            :total="tableData.length">
          </el-pagination>
        </div></el-col>

        <el-col :span="1"><div class="grid-content">
          <el-divider direction="vertical" ></el-divider>
        </div></el-col>

        <el-col :span="8"><div class="grid-content">
          <div id="myChart" :style="{width: '100%', height: '400px'}"></div>
        </div></el-col>
      </el-row>
    </el-card>
  </div>
</template>

<script>
export default {
  data() {
    return {
      database:{
        mysqlbTime: 0,
        mysqlaTime: 0,
        hiveTime: 0,
        neo4jTime: 0,
      },
      form: {
        time: {
          year: '',
          month: '',
          quarter: '',
        },
      },
      formMark:{
        time: 'year',
      },
      tableData: [{
            "movie_id": 27,
            "title": "Will You Still Love Me Tomorrow?",
            "runtime": 106,
            "releasedate": "2014-07-08"
        },
        {
            "movie_id": 33,
            "title": "INXS - Mystify",
            "runtime": 105,
            "releasedate": "2014-03-17"
        },
        {
            "movie_id": 36,
            "title": "Aftermath",
            "runtime": 104,
            "releasedate": "2014-10-07"
        }]
    }
  },
  computed:{
    formMonthMark(){
      return this.formMark.time=="month";
    },
    formQuarterMark(){
      return this.formMark.time=="quarter";
    },
  },
  methods: {
    onSubmit() {
      if(this.formMark.time=='year'){
        this.$axios
          .get("/getMoviesByYearFromD1", {
            params: {
              year: this.form.time.year
            }
          })
          .then((response)=>{
            this.tableData = response.data.data;
            this.database.mysqlbTime=response.data.time;
            this.draw();
          })
        this.$axios
          .get("/getMoviesByYearFromD2", {
            params: {
              year: this.form.time.year
            }
          })
          .then((response)=>{
            this.database.mysqlaTime=response.data.time;
            this.draw();
          })
      }
      else if(this.formMark.time=='month'){
        this.$axios
          .get("/getMoviesByYMFromD1", {
            params: {
              year: this.form.time.year,
              month: this.form.time.month
            }
          })
          .then((response)=>{
            this.tableData = response.data.data;
            this.database.mysqlbTime=response.data.time;
            this.draw();
          })
        this.$axios
          .get("/getMoviesByYMFromD2", {
            params: {
              year: this.form.time.year,
              month: this.form.time.month
            }
          })
          .then((response)=>{
            this.tableData = response.data.data;
            this.database.mysqlaTime=response.data.time;
            this.draw();
          })
      }
      else if(this.formMark.time=='quarter'){
        this.$axios
          .get("/getMoviesByYQFromD1", {
            params: {
              year: this.form.time.year,
              quarter: this.form.time.quarter
            }
          })
          .then((response)=>{
            this.tableData = response.data.data;
            this.database.mysqlbTime=response.data.time;
            this.draw();
          })
        this.$axios
          .get("/getMoviesByYQFromD2", {
            params: {
              year: this.form.time.year,
              quarter: this.form.time.quarter
            }
          })
          .then((response)=>{
            this.database.mysqlaTime=response.data.time;
            this.draw();
          })
      }
      
    },
    draw(){
      // 初始化echarts实例
      let myChart = this.$echarts.init(document.getElementById('myChart'))
      // 指定图表的配置项和数据
      var option = {
        title: {
          text: ''
        },
        tooltip: {},
        legend: {
          data: ['查询时间']
        },
        xAxis: {
          data: ['MySQL', 'MySQL(优化后)', 'HIVE']
        },
        yAxis: {},
        series: [
          {
            name: '查询时间',
            type: 'line',
            data: [this.database.mysqlbTime, this.database.mysqlaTime, this.database.hiveTime]
          }
        ]
      };
      //防止越界，重绘canvas
      window.onresize = myChart.resize;
      myChart.setOption(option);//设置option
    }
  },
  mounted(){
    this.draw();
  }
}
</script>

<style lang="scss" scoped>
  .grid-content {
    border-radius: 4px;
    min-height: 36px;
  }
  .el-divider--vertical{
    display: inline-block;
    width: 1px;
    height: 31em;
    margin: 0 8px;
    vertical-align: middle;
    position: relative;
  } 
</style>
