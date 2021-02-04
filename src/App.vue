<template>
  <div id="app">
    <form>
      <label for="lang"><span>选择翻译成:</span></label>
      <select name="lang" id="lang" v-model="dataval">
        <option value="zh" selected>中文</option>
        <option value="en">英文</option>
        <option value="jp">日文</option>
      </select>
      <br />
      <textarea name="query" id="input_v" cols="30" rows="10"></textarea>
      <input @click="btn" type="button" value="翻译" />
      <textarea name="query" id="translate" cols="30" rows="10"></textarea>
    </form>
  </div>
</template>

<script>
import MD5 from "md5";
import $ from "jquery";
export default {
  name: "App",
  components: {},
  data() {
    return {
      dataval: "",
    };
  },
  methods: {
    btn() {
      let q = document.querySelector("#input_v").value;
      let appid = "20180427000151308";
      let key = "ohOSSCROw_oNxIRudFGQ";
      let salt = new Date().getTime();
      let query = q;
      console.log(query);
      // 多个query可以用\n连接  如 query='apple\norange\nbanana\npear'
      let from = "auto";
      let to = this.dataval;
      let str1 = appid + query + salt + key;
      let sign = MD5(str1);
      // this.$axios
      //   .get("http://api.fanyi.baidu.com/api/trans/vip/translate", {
      //     q: query,
      //     appid: appid,
      //     salt: salt,
      //     from: from,
      //     to: to,
      //     sign: sign,
      //   })
      //   .then((res) => {
      //     document.querySelector("#translate").value = res.trans_result[0].dst;
      //   });
      $.ajax({
        url: "http://api.fanyi.baidu.com/api/trans/vip/translate",
        type: "get",
        dataType: "jsonp",
        data: {
          q: query,
          appid: appid,
          salt: salt,
          from: from,
          to: to,
          sign: sign,
        },
        success: function (data) {
          console.log(data);
          console.log(data.trans_result[0].dst);
          document.querySelector("#translate").value = data.trans_result[0].dst;
        },
      });
    },
  },
};
</script>

<style lang="less">
* {
  padding: 0;
  margin: 0;
  #app {
    width: 800px;
    height: 300px;
    margin: 60px auto;
    color: #2c3e50;
    margin-top: 60px;
    textarea {
      font-size: 18px;
    }
    input {
      width: 60px;
      height: 30px;
    }
  }
}
</style>
