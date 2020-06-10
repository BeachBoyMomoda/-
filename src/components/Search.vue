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
      <el-form-item label="热门">
        <el-tag
          style="margin-left: 10px"
          :key="tag"
          v-for="tag in hotFilms"
          @click="handleSelect(tag)">
          {{tag}}
        </el-tag>
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
      suggestList: [],
      tagOptions: ['剧情','爱情','恐怖','战争','喜剧'],
      locationOptions: ['中国','港台','日本','韩国','美国'],
      hotFilms: [],
      paramStr: ''
    }
  },
  created () {
    this.fetchHot()
  },
  methods: {
    fetchHot () {
      this.$http.get('http://39.96.37.67:8080/hotSearchFilmTitle',{
        params:{
          ends: 5,
          start:0
        }
      }).then(res=>{
        this.hotFilms = res.body
      })
    },
    handleSearch: function() {
      let paramStr = '?lowRating=' + this.postForm.rating[0] + '&toRating=' + this.postForm.rating[1]
      if (this.postForm.title !== '') {
        paramStr += '&title=' + this.postForm.title
      }
      if (this.postForm.actors !== '') {
        paramStr += '&actors=' + this.postForm.actors
      }
      if (this.postForm.director !== '') {
        paramStr += '&author=' + this.postForm.director
      }
      if (this.postForm.summary !== '') {
        paramStr += '&summary=' + this.postForm.summary
      }
      for(let tag in this.tags){
        this.postForm.tags += ',' + tag
      }
      if (this.postForm.tags !== '') {
        paramStr += '&tags=' + this.postForm.tags
      }
      if (this.postForm.location !== '') {
        paramStr += '&location=' + this.postForm.location
      }
      this.$router.push({path:'/result', query: {paramStr}})
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
    },
    handleSelect (tag) {
      this.postForm.title = tag
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
