<template>
  <div class="app-container">
    <el-card style="width:100%; margin:0 auto; padding-top: 20px">
      <el-form ref="form" :model="form" label-width="120px">
        <el-row>
          <el-col :span="7" :push='1'><div class="grid-content bg-purple-dark">
            <el-radio v-model="radio" label="1">经常一起合作的演员</el-radio>
          </div></el-col>
          <el-col :span="7" :push='1'><div class="grid-content bg-purple-dark">
            <el-radio v-model="radio" label="4">某一演员与其他演员的合作关系</el-radio>
          </div></el-col>
          <el-col :span="7" :push='1'><div class="grid-content">
            <el-input v-model="form.actor" v-if="radio==4" placeholder="请输入演员名称" />
          </div></el-col>
        </el-row>          
        <el-row>
          <el-col :span="7" :push='1'><div class="grid-content">
            <el-radio v-model="radio" label="2">经常一起合作的导演和演员</el-radio>
          </div></el-col>
          <el-col :span="7" :push='1'><div class="grid-content">
            <el-radio v-model="radio" label="5">某一演员与其他导演的合作关系</el-radio>
          </div></el-col>
          <el-col :span="7" :push='1'><div class="grid-content">
            <el-input v-model="form.actor" v-if="radio==5" placeholder="请输入演员名称" />
          </div></el-col>
        </el-row>
        <el-row>
          <el-col :span="7" :push='1'><div class="grid-content">
            <el-radio v-model="radio" label="3">经常一起合作的编剧和演员</el-radio>
          </div></el-col>
          <el-col :span="7" :push='1'><div class="grid-content">
            <el-radio v-model="radio" label="6">某一演员与其他编剧的合作关系</el-radio>
          </div></el-col>
          <el-col :span="7" :push='1'><div class="grid-content">
            <el-input v-model="form.actor" v-if="radio==6" placeholder="请输入演员名称" />
          </div></el-col>
        </el-row> 
        <el-row>
          <el-col :span="24" :push='22'><div class="grid-content">
            <el-button type="primary" @click="onSubmit">查询</el-button>
          </div></el-col>
        </el-row>
        
        
        
      </el-form>
    </el-card>
    <el-card style="width:100%; margin:2% auto; height: 410px;">
      <el-row>
        <el-col :span="12"><div class="grid-content">
          <el-table
            :data="tableData"
            :stripe="true"            
            height="330"
            :highlight-current-row='true'
            style="width: 95%; margin: 0; height: 330px;">
            <el-table-column
              prop="name1"
              label="演员"
              align='center'
              width="200">
            </el-table-column>
            <el-table-column
              prop="name2"
              label="演员"
              align='center'
              width="200"
              v-if="radiolabel==1||radiolabel==4">
            </el-table-column>
            <el-table-column
              prop="name2"
              label="导演"
              align='center'
              width="200"
              v-if="radiolabel==2||radiolabel==5">
            </el-table-column>
            <el-table-column
              prop="name2"
              label="编剧"
              align='center'
              width="200"
              v-if="radiolabel==3||radiolabel==6">
            </el-table-column>
            <el-table-column
              prop="count"
              label="合作次数"
              align='center'
              width="200">
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

        <el-col :span="11"><div class="grid-content">
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
      form:{
        actor:''
      },
      tableData: [],
      radio: '',
      radiolabel: 1
    }
  },
  methods: {
    onSubmit() {
      if(this.radio==1){
        this.$axios
          .get("/getTopCoopAA")
          .then((response)=>{
            this.tableData = response.data.data;
            this.database.neo4jTime=response.data.time;
            this.draw();
          })
        this.$axios
          .get("/getCoopAATopFromD2")
          .then((response)=>{
            this.database.mysqlaTime=response.data.time;
            this.draw();
          })
        this.$axios
          .get("/getCoopAATopFromD1")
          .then((response)=>{
            this.database.mysqlbTime=response.data.time;
            this.draw();
          })
        this.radiolabel=1;
      }
      else if(this.radio==2){
        this.$axios
          .get("/getTopCoopAD")
          .then((response)=>{
            this.tableData = response.data.data;
            this.database.neo4jTime=response.data.time;
            this.draw();
          })
        this.$axios
          .get("/getCoopADTopFromD2")
          .then((response)=>{
            this.database.mysqlaTime=response.data.time;
            this.draw();
          })
        this.$axios
          .get("/getCoopADTopFromD1")
          .then((response)=>{
            this.database.mysqlbTime=response.data.time;
            this.draw();
          })
        this.radiolabel=2;
      }
      else if(this.radio==3){
        this.$axios
          .get("/getTopCoopAW")
          .then((response)=>{
            this.tableData = response.data.data;
            this.database.neo4jTime=response.data.time;
            this.draw();
          })
        this.$axios
          .get("/getCoopAWTopFromD2")
          .then((response)=>{
            this.database.mysqlaTime=response.data.time;
            this.draw();
          })
        this.$axios
          .get("/getCoopAWTopFromD1")
          .then((response)=>{
            this.database.mysqlbTime=response.data.time;
            this.draw();
          })
        this.radiolabel=3;
      }
      else if(this.radio==4){
        this.$axios
          .get("/getTopCoopAAByName",{
            params: {
              name:this.form.actor
            }
          })
          .then((response)=>{
            this.tableData = response.data.data;
            this.database.neo4jTime=response.data.time;
            this.draw();
          })
        this.$axios
          .get("/getCoopAAByActorFromD2",{
            params: {
              actor:this.form.actor
            }
          })
          .then((response)=>{
            this.database.mysqlaTime=response.data.time;
            this.draw();
          })
        this.$axios
          .get("/getCoopAAByActorFromD1",{
            params: {
              actor:this.form.actor
            }
          })
          .then((response)=>{
            this.database.mysqlbTime=response.data.time;
            this.draw();
          })
        this.radiolabel=4;
      }
      else if(this.radio==5){
        this.$axios
          .get("/getTopCoopADByName",{
            params: {
              name:this.form.actor
            }
          })
          .then((response)=>{
            this.tableData = response.data.data;
            this.database.neo4jTime=response.data.time;
            this.draw();
          })
        this.$axios
          .get("/getCoopADByActorFromD2",{
            params: {
              actor:this.form.actor
            }
          })
          .then((response)=>{
            this.database.mysqlaTime=response.data.time;
            this.draw();
          })
        this.$axios
          .get("/getCoopADByActorFromD1",{
            params: {
              actor:this.form.actor
            }
          })
          .then((response)=>{
            this.database.mysqlbTime=response.data.time;
            this.draw();
          })
        this.radiolabel=5;
      }
      else if(this.radio==6){
        this.$axios
          .get("/getTopCoopAWByName",{
            params: {
              name:this.form.actor
            }
          })
          .then((response)=>{
            this.tableData = response.data.data;
            this.database.neo4jTime=response.data.time;
            this.draw();
          })
        this.$axios
          .get("/getCoopAWByActorFromD2",{
            params: {
              actor:this.form.actor
            }
          })
          .then((response)=>{
            this.database.mysqlaTime=response.data.time;
            this.draw();
          })
        this.$axios
          .get("/getCoopAWByActorFromD1",{
            params: {
              actor:this.form.actor
            }
          })
          .then((response)=>{
            this.database.mysqlbTime=response.data.time;
            this.draw();
          })
        this.radiolabel=6;
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
          data: ['MySQL', 'MySQL(优化后)', 'HIVE', 'neo4j']
        },
        yAxis: {},
        series: [
          {
            name: '查询时间',
            type: 'line',
            data: [this.database.mysqlbTime, this.database.mysqlaTime, this.database.hiveTime, this.database.neo4jTime]
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
