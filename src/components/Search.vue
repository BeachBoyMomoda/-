<template>
  <div>
    <!--监听子组件的时间-->
    <img src="../static/img/logo.png" />
    <el-form
      ref="postForm"
      :model="postForm"
      :rules="rules"
      size="medium"
      label-position="right"
      label-width="120px"
      class="main_container">
      <el-form-item label="片名">
        <el-row :gutter="0">
          <el-col :span="21">
            <el-popover placement="bottom-start" v-model="visible" trigger="manual">
              <ul>
                <li v-for="item in suggestList" :label="item" :key="item" @click="suggestItemClick(item)">{{item}}</li>
              </ul>
              <div slot="reference">
                <el-input placeholder="请输入内容" v-model="postForm.title" @input="handleSuggest" @focus="visible=true" @blur="visible=false"></el-input>
              </div>
            </el-popover>
          </el-col>
          <el-col :span="2">
            <el-button type="primary" @click="handleSearch">搜索</el-button>
          </el-col>
        </el-row>
      </el-form-item>
      <el-form-item label="导演">
        <el-input v-model="postForm.director" />
      </el-form-item>
      <el-form-item label="主演">
        <el-input v-model="postForm.actors" />
      </el-form-item>
      <el-form-item label="简介">
        <el-input v-model="postForm.summary" />
      </el-form-item>
      <el-form-item label="标签">
        <el-row :gutter="0">
          <el-col :span="15">
            <el-checkbox-group v-model="tags" size="small">
              <el-checkbox-button v-for="tag in tagOptions" :label="tag" :key="tag">{{tag}}</el-checkbox-button>
            </el-checkbox-group>
          </el-col>
          <el-col :span="8">
            <el-input v-model="postForm.tags" placeholder="其他..." />
          </el-col>
        </el-row>
      </el-form-item>
      <el-form-item label="地区">
        <el-row :gutter="0">
          <el-col :span="15">
            <el-radio-group v-model="postForm.location" size="small">
              <el-radio-button v-for="location in locationOptions" :label="location" :key="location">{{location}}</el-radio-button>
            </el-radio-group>
          </el-col>
          <el-col :span="8">
            <el-input v-model="postForm.location" placeholder="其他..." />
          </el-col>
        </el-row>
      </el-form-item>
      <el-form-item label="评分范围">
        <el-slider
          v-model="postForm.rating"
          range
          show-stops
          :max="10">
        </el-slider>
      </el-form-item>
    </el-form>
    <el-table v-if="searchResult.length>0" :data="searchResult" size="medium" class="table_container">
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
      <el-table-column label="导演" prop="director" />
      <el-table-column label="演员" prop="actors" width="300px"/>
      <el-table-column label="简介" prop="summary" width="300px"/>
      <el-table-column label="标签" prop="tags" />
      <el-table-column label="评分" prop="rating" width="80px" />
    </el-table>
  </div>
</template>

<script>
export default {
  data: function () {
    return {
      postForm: {
        title: '',
        director: '',
        actors: '',
        tags: '',
        summary: '',
        location: '',
        rating: [0, 10]
      },
      visible: false,
      rules: {},
      tags: [],
      searchResult: [],
      suggestList: [],
      tagOptions: ['剧情','爱情','恐怖','战争','喜剧'],
      locationOptions: ['中国','港台','日本','韩国','美国']
    }
  },
  methods: {
    handleSearch () {
      let params={
        lowRating: this.postForm.rating[0],
        toRating: this.postForm.rating[1]
      }
      if (this.postForm.title !== '') {
        params['title']= this.postForm.title
      }
      if (this.postForm.actors !== '') {
        params['actors'] =this.postForm.actors
      }
      for(let tag in this.tags){
        this.postForm.tags += ',' + tag
      }
      if (this.postForm.tags !== '') {
        params['tags'] =this.postForm.tags
      }

      if (this.postForm.location !== '') {
        params['location'] =this.postForm.location
      }
      this.$http.get('http://39.96.37.67:8080/match', {
        params: params
      }).then(res => {
        this.searchResult = res.body
      })
    },
    handleSuggest () {
      console.log('suggest!!!!!!!!!!!!')
      this.$http.get('http://39.96.37.67:8080/getSuggest',{
        params: {
          suggestField: 'titleSuggest',
          suggestValue: this.postForm.title
        }
      }).then(res=>{
        this.suggestList = res.body
      })
    },
    suggestItemClick(item) {
      this.postForm.title = item
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style type="text/css">
.main_container {
  height: 100%;
  position: absolute;
  top: 160px;
  left: 400px;
  width: 40%;
}
.table_container {
  position: absolute;
  top: 600px;
  left: 0px;
  width: 100%;
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
