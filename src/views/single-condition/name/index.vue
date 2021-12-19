<template>
  <div class="app-container">
    <el-card style="width:100%; margin:0 auto; padding-top: 20px">
      <el-form ref="form" :model="form" label-width="120px">
        <el-form-item label="电影名">
          <el-row>
            <el-col :span="18"><div class="grid-content">
              <el-autocomplete
                class="inline-input"
                v-model="form.name"
                :fetch-suggestions="querySearch"
                placeholder="请输入电影名"
                @select="handleSelect"
                clearable
                style="width:100%"
              ></el-autocomplete>
            </div></el-col>
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
        name:''
      },
      formMark:{
        actor: 'actor',
      },
      tableData: [],
      names:[]
    }
  },
  methods: {
    onSubmit() {
      this.$axios
        .get("/getMoviesByTitleFromD1", {
          params: {
            title:this.form.name
          }
        })
        .then((response)=>{
          this.tableData = response.data.data;
          this.database.mysqlbTime=response.data.time;
          this.draw();
        })
      this.$axios
        .get("/getMoviesByTitleFromD2", {
          params: {
            title:this.form.name
          }
        })
        .then((response)=>{
          this.database.mysqlaTime=response.data.time;
          this.draw();
        })
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
    },
    querySearch(queryString, cb) {
      var names = this.names;
      var results = queryString ? names.filter(this.createFilter(queryString)) : names;
      // 调用 callback 返回建议列表的数据
      cb(results);
    },
    createFilter(queryString) {
      return (name) => {
        return (name.value.toLowerCase().indexOf(queryString.toLowerCase()) === 0);
      };
    },
    loadAll() {
      this.$axios
        .get("getMoviesByTitleFromD1",{
            params: {
              title:''
            }
        })
        .then((response)=>{
          this.names=[];
            for(let i = 0; i < response.data.data.length; ++i){
              this.names.push({
                "value":  response.data.data[i].title
              })
            }
          })
    },
  },
  mounted(){
    this.draw();
    this.loadAll();
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
