<template>
  <div>
    <div class="header">
      <ul class="header-button-left">
        <li>Cancel</li>
      </ul>
      <ul class="header-button-right">
        <li v-if="step==1" @click="step++">Next</li>
        <li v-if="step==2" @click="publish">발행</li>
      </ul>
      <img src="./assets/logo.png" class="logo" />
    </div>

<!--    <h4>안녕 {{ $store.state.name }}</h4>-->
<!--    <h4>내나이 {{$store.state.age}}</h4>-->
<!--    <button @click="$store.commit('이름변경')">버튼</button>-->
<!--    <button @click="나이변경(10)">버튼</button>-->

<!--    <p>{{작명}} {{age}} {{likes}}</p>-->

<!--    <p>{{$store.state.more}}</p>-->
<!--    <button @click="$store.dispatch('getData')">더보기버튼</button>-->

    <Container @write="작성한글 = $event" :이미지="이미지" :게시물="게시물" :step="step"/>
<!--    <button @click="more">더보기</button>-->

    <div class="footer">
      <ul class="footer-button-plus">
        <input @change="upload" multiple type="file" id="file" class="inputfile" />
        <label for="file" class="input-plus">+</label>
      </ul>
    </div>

<!--    <div v-if="step == 0">내용0</div>-->
<!--    <div v-if="step == 1">내용1</div>-->
<!--    <div v-if="step == 2">내용2</div>-->
<!--    <button @click="step = 0">버튼0</button>-->
<!--    <button @click="step = 1">버튼1</button>-->
<!--    <button @click="step = 2">버튼2</button>-->
  </div>

</template>

<script>
import Container from "@/components/Container";
import postdata from  './assets/postdata';
import axios from 'axios';
import {mapMutations,mapState} from 'vuex';
axios.get()

export default {
  name: 'App',
  data(){
    return {
      게시물 : postdata,
      더보기 : 0,
      step : 3,
      이미지 : '',
      작성한글 : '',
      선택한필터: '',
    }
  },
  mounted(){
    this.emitter.on('박스클릭함', (a)=>{
      this.선택한필터 = a
    })
  },
  components: {
    Container,
  },

  computed : {
    name(){
      return this.$store.state.name
    },
    ...mapState(['name','age','likes']),
    ...mapState({ 작명 : 'name' }),
  },

  methods : {
    ...mapMutations(['setMore','좋아요','나이변경']),

    publish(){
      var 내게시물 ={
        name: "Kim Hyun",
            userImage: "https://placeimg.com/100/100/arch",
            postImage: this.이미지,
            likes: 36,
            date: "May 15",
            liked: false,
            content: this.작성한글,
            filter: this.선택한필터
      };
      this.게시물.unshift(내게시물);
      this.step = 0;
    },
    more(){
      axios.get(`https://codingapple1.github.io/vue/more${this.더보기}.json`)
          .then( 결과 => {
            this.게시물.push(결과.data);
            this.더보기++;
      })
    },
    upload(e){
      let 파일 = e.target.files;
      console.log(파일[0]);
      let url = URL.createObjectURL(파일[0]);
      console.log(url);
      this.이미지=url;
      this.step++;
    }
  },
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
body {
  margin: 0;
}
ul {
  padding: 5px;
  list-style-type: none;
}
.logo {
  width: 22px;
  margin: auto;
  display: block;
  position: absolute;
  left: 0;
  right: 0;
  top: 13px;
}
.header {
  width: 100%;
  height: 40px;
  background-color: white;
  padding-bottom: 8px;
  position: sticky;
  top: 0;
}
.header-button-left {
  color: skyblue;
  float: left;
  width: 50px;
  padding-left: 20px;
  cursor: pointer;
  margin-top: 10px;
}
.header-button-right {
  color: skyblue;
  float: right;
  width: 50px;
  cursor: pointer;
  margin-top: 10px;
}
.footer {
  width: 100%;
  position: sticky;
  bottom: 0;
  padding-bottom: 10px;
  background-color: white;
}
.footer-button-plus {
  width: 80px;
  margin: auto;
  text-align: center;
  cursor: pointer;
  font-size: 24px;
  padding-top: 12px;
}
.sample-box {
  width: 100%;
  height: 600px;
  background-color: bisque;
}
.inputfile {
  display: none;
}
.input-plus {
  cursor: pointer;
}
#app {
  box-sizing: border-box;
  font-family: "consolas";
  margin-top: 60px;
  width: 100%;
  max-width: 460px;
  margin: auto;
  position: relative;
  border-right: 1px solid #eee;
  border-left: 1px solid #eee;
}
</style>
