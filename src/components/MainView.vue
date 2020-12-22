<template>
  <div class="main">
    <div class="header">百度在线翻译</div>
    <div class="container">
      <div class="box">
        <div class="in">
          <input v-model="query" type="text" />
        </div>
        <div class="lang">
          <select @change="getValue" v-model="selected">
            <option v-for="(item,index) in transArr" :value="item.lang" :key="index">{{item.name}}</option>
          </select>
        </div>
        <div>
          <button @click="getRes">翻译</button>
        </div>
        <div v-show="res1===''?false:true" class="result">{{res1}}</div>
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios";
import MD5 from "../assets/md5";
import Vue from "vue";
export default {
  data() {
    return {
    
      selected: "auto",
      transArr: [
        { name: "英语", lang: "en" },
        { name: "韩语", lang: "kor" },
        { name: "日语", lang: "jp" },
        { name: "自动检测", lang: "auto" },
       
        {name:'文言文',lang:'wyw'},
        {name:'西班牙语',lang:'spa'},
        {name:'中文',lang:'zh'},
        {name:'繁体中文',lang:'cht'}
      ],
      appid: "20201222000652993",
      keys: "KBAPexVsyOFTpMLbdzUf",
      salt: new Date().getTime(),
      query: "苹果",
      from: "zh",
      to: "jp",
      str1: "",
      sign: "",
      res1: "",
    };
  },
  created() {

  },

  methods: {
      getValue(){
          this.getRes()
      },
    getStr1() {
      this.str1 = this.appid + this.query + this.salt + this.keys;
      this.sign = MD5(this.str1);
      console.log(this.sign);
    },
    getRes() {
      this.getStr1();
      let vm = this;
      $.ajax({
        url: "http://api.fanyi.baidu.com/api/trans/vip/translate",
        type: "get",
        dataType: "jsonp",
        data: {
          q: this.query,
          appid: this.appid,
          salt: this.salt,
          from: this.from,
          to: this.selected,
          sign: this.sign,
        },
        success: function (data) {
          vm.res1 = data.trans_result[0].dst;
          console.log(vm.res1);
        },
        error:function(data){
            console.log(data)
        }
      });
      this.getAudio();
    },
    getAudio(){
            this.getStr1();
          let vm = this
   $.ajax({
        url: "http://api.fanyi.baidu.com/api/trans/vip/language",
        type: "get",
        dataType: "jsonp",
        data: {
          q: this.query,
          appid: this.appid,
          salt: this.salt,
          sign: this.sign,
        },
        success: function (data) {
        //   vm.res1 = data.trans_result[0].dst;
        //   console.log(vm.res1);
        console.log(data)
        },
        error:function(data){
            console.log(data)
        }
      });
    }
  },
};
</script>

<style>
.header {
  background-color: #3498db;
  text-align: center;
  width: 100%;
  height: 40px;
  line-height: 40px;
  color: #fff;
  font-weight: 400;
}
.container {
  width: 90%;
  margin: 0 auto;
}
.box {
  width: 100%;
  margin-top: 1.25rem;
  height: 25rem;
  display: flex;
  flex-direction: column;
  align-items: start;
}
.box > div {
  height: 40px;
  line-height: 40px;
  margin-bottom: 20px;
  width: 100%;
}
.in input {
  width: 100%;
  height: 100%;
  text-indent: 1rem;
  outline: none;
  font-size: 1.25rem;
  color: #777;
  border: 1px solid #ddd;
}
.lang select {
  text-indent: 1rem;
  display: block;
  width: 100%;
  height: 100%;
  font-size: 1.25rem;
  outline: none;
  border: 1px solid #ddd;
}
button {
  width: 100%;
  height: 100%;
  outline: none;
  border: none;
  line-height: 40px;
  background-color: #3498db;
  color: #fff;
  cursor: pointer;
}
.box .result {
  text-indent: 1rem;
  width: 100%;
  background-color: #fff;
}
</style>