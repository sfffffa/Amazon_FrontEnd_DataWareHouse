<template>
  <div>
      <Card>
        <p slot="title" class="card-title">
            <Icon type="home"></Icon>
            <span>数据仓库大项目</span>
        </p>
        <div>
          <h2>数据来源:</h2>
          <div class="margin-bottom-10" style="margin-left:20px">
            <ul>
              <li>Stanford:<a target="_blank" href="http://snap.stanford.edu/data/web-Movies.html">http://snap.stanford.edu/data/web-Movies.html</a></li>
            </ul>
          </div>
          <br>
          <h2>项目要求:</h2>
          <div class="margin-bottom-10" style="margin-left:20px">
            <ul>
              <li>针对电影及其周边信息，参考数据使用场景，建立至少2种存储模型，设计其存储架构并进行系统性能比对。</li>
              <li>i.	能够从数据来源处获取数据</li>
              <li>ii.	参考数据使用场景，建立适当的存储模型存储数据</li>
              <li>iii.	在数据展现的界面上能够进行典型数据使用场景中的查询，并将在多种不同存储模型上的执行性能以数值的方式和图表的方式显示在界面上</li>
              <li>iv.	撰写文档说明数据存储模型选择的理由、数据存储模型、系统架构。</li>
            </ul>
          </div>
          <br>
          <h2>数据来源:</h2>
          <div class="margin-bottom-10" style="margin-left:20px">
              <ul>
                <li>数据来源自Snap的文本文件和Amazon网站，数据包括但不限于以下信息：</li>
                电影ID，评论用户ID，评论用户ProfileName，评论用户评价Helpfulness，评论用户Score，评论时间Time，评论结论Summary，<br>
                评论结论Text，电影演员，电影上映时间，电影风格，电影导演，电影主演，电影演员，电影版本等信息。
              </ul>
          </div>
          <br>
          <h2>数据使用场景:</h2>
          <div class="margin-bottom-10" style="margin-left:20px">
              <ul>
                <li>i.	按照时间进行查询及统计（例如XX年有多少电影，XX年XX月有多少电影，XX年XX季度有多少电影，周二新增多少电影等）</li>
                <li>ii.	按照电影名称进行查询及统计（例如 XX电影共有多少版本等）</li>
                <li>iii.	按照导演进行查询及统计（例如 XX导演共有多少电影等）</li>
                <li>v.	按照电影类别进行查询及统计（例如 Action电影共有多少，Adventure电影共有多少等）</li>              
                <li>vi.	按照导演-演员的合作关系，或者演员-演员的合作关系进行查询（例如XX演员经常和哪些演员及导演合作，合作了哪些作品；或哪些演员经常一起合作等））</li>  
                <li>vii.	按照电影上映时间进行查询及统计（例如Action类型电影常见的黄金上映期是什么？恐怖片和动作片的排片日期有没有什么关系等）</li>  
                <li>viii.	按照上述条件的组合查询和统计</li>  

              </ul>
          </div>
          <br>
        </div>
      </Card>
  </div>
</template>

<script>
export default {
}
</script>