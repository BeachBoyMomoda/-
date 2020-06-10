<template>
  <div>
    <el-button type="primary" icon="el-icon-refresh-right" class="reset_button_container" @click="handleReset">重新搜索</el-button>
    <el-pagination
      :current-page.sync="pageNo"
      layout="prev, pager, next"
      :total="total"
      :page-size="20"
      @current-change="fetch">
    </el-pagination>
    <el-table stripe :data="searchResult" size="medium">
      <el-table-column label="片名" prop="title" width="160px">
        <template slot-scope="scope">
          <el-row>
            <el-col>
              {{scope.row.title}}
            </el-col>
            <el-col>
              <img :src="scope.row.imagePath" width="100px"  alt="加载失败"/>
            </el-col>
          </el-row>
        </template>
      </el-table-column>
      <el-table-column label="上映日期" prop="releaseDate" width="120px" />
      <el-table-column label="地区" prop="location" width="80px" />
      <el-table-column label="导演" prop="director" width="80px"/>
      <el-table-column label="演员" prop="actors"/>
      <el-table-column label="简介" prop="summary"/>
      <el-table-column label="标签" prop="tags" width="150px"/>
      <el-table-column label="评分" prop="rating" width="80px" />
    </el-table>
    <el-pagination
      :current-page.sync="pageNo"
      layout="prev, pager, next"
      :total="total"
      :page-size="20"
      @current-change="fetch">
    </el-pagination>
  </div>
</template>

<script>
export default {
  data: function () {
    return {
      searchResult: [],
      total: 0,
      pageNo: 1,
      query: ''
    }
  },
  created () {
    this.query=this.$route.query.paramStr
    this.fetch()
  },
  methods: {
    fetch () {
      this.query += '&page=' + (this.pageNo - 1)
      this.$http.get('http://39.96.37.67:8080/match' + this.query).then(res => {
        this.searchResult = res.body.content
        this.total=res.body.totalElements
        for (let i = 0; i < this.searchResult.length; ++i) {
          let reg = new RegExp( '\', \'' , "g" )
          this.searchResult[i].actors = this.searchResult[i].actors.substring(3,this.searchResult[i].actors.length-3).replace(reg, ',')
          this.searchResult[i].director = this.searchResult[i].director.substring(2,this.searchResult[i].director.length-2).replace(reg, ',')
          this.searchResult[i].tags = this.searchResult[i].tags.substring(3,this.searchResult[i].tags.length-3).replace(reg, ',')
        }
      })
    },
    handleReset () {
      this.$router.push({path:'/'})
    }
  }
}
</script>

<style type="text/css">
.main_container {
  height: 100%;
  position: absolute;
  top: 160px;
  left: 500px;
  width: 40%;
}
.reset_button_container{
  position: absolute;
  top: 0px;
  left: 100px;
  width: 6%;
}
.table_container {
  position: absolute;
  top: 60px;
  left: 0px;
  width: 100%;
}
.black_label {
  color: yellow;
}
.search-input input {
  border: 1px solid #e4e4e4;
  box-sizing: border-box;
  width: 500px;
  height: 45px;
  float: left;
  padding-left: 10px;
  padding-right: 10px;
  font-size: 18px;
  overflow: hidden;
}
.search-select li {
  border: 1px solid #d4d4d4;
  border-top: none;
  border-bottom: none;
  background-color: #fff;
  width: 100%
}
.search-select ul{
  margin:0;
  text-align: left;
}
</style>
