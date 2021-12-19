<template>
  <div class="app-container">
    <el-card style="width:100%; margin:0 auto; padding-top: 20px">
      <el-form ref="form" :model="form" label-width="120px">
        <el-form-item label="电影类别">
          <el-row>
            <el-col :span="18"><div class="grid-content">
              <el-autocomplete
                class="inline-input"
                v-model="form.genre"
                :fetch-suggestions="querySearch"
                placeholder="请输入电影类别"
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
            :highlight-current-row='true'
            style="width: 95%; margin: 0; height: 330px;">
            <el-table-column
              prop="name"
              label="电影名称"
              align='center'
              width="180">
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
      form: {
        genre:''
      },
      tableData: [{
        name: 'A电影',
        }, {
        name: 'B电影',
        },{
        name: 'C电影',
      },],
      genres:[],
    }
  },
  methods: {
    onSubmit() {
      this.$message('submit!')
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
          data: ['MySQL', 'HIVE', 'Neo4j']
        },
        yAxis: {},
        series: [
          {
            name: '查询时间',
            type: 'line',
            data: [0.03, 0.13, 0.5]
          }
        ]
      };
      //防止越界，重绘canvas
      window.onresize = myChart.resize;
      myChart.setOption(option);//设置option
    },
    querySearch(queryString, cb) {
      var genres = this.genres;
      var results = queryString ? genres.filter(this.createFilter(queryString)) : genres;
      // 调用 callback 返回建议列表的数据
      cb(results);
    },
    createFilter(queryString) {
      return (genre) => {
        return (genre.value.toLowerCase().indexOf(queryString.toLowerCase()) === 0);
      };
    },
    loadAll() {
      return [
        { "value": "Action" },
        { "value": "Adventure",},
      ];
    },
    handleSelect(item) {
      console.log(item);
    },
  },
  mounted(){
    this.draw();
    this.genres = this.loadAll();
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
