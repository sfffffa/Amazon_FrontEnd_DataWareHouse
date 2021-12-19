<template>
  <div class="app-container">
    <el-card style="width:70%; margin:0 auto; height:90%" >
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
          </el-row>        
        </el-form-item>

        <el-form-item>
          <el-button type="primary" @click="onSubmit" >查询</el-button>
          <!-- <el-button @click="onCancel">Cancel</el-button> -->
        </el-form-item>
      </el-form>
    </el-card>
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
        genre:'',
      },
      formMark:{
        time: 'year',
        actor: 'actor',
      },
      genres:[],

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
    // onCancel() {
    //   this.$message({
    //     message: 'cancel!',
    //     type: 'warning'
    //   })
    // },
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
  mounted() {
    this.genres = this.loadAll();
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
  }rid-content {
    border-radius: 4px;
    min-height: 36px;
  }
</style>
