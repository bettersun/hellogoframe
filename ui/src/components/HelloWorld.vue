<template>
  <div class="hello">
    <div class="list-group" id="divShow"></div>
    <div>
        <div><input class="form-control" v-model="message" autofocus rows="6" placeholder="请输入发送内容" @keydown.enter="handleMessage()"></div>
        <div><button class="btn btn-default" style="margin-top:15px" @click="sendMessage()">发 送</button></div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'HelloWorld',
  props: {
    msg: String
  },
  data() {
    return {
      ws: null,
      url: '',
      message: '',
    }
  },
  created() {
    console.log("created")
    this.initWebSocket()
  },
  methods: {
    //  初始化WebSocket
    initWebSocket() {
        this.url = "ws://127.0.0.1:8199/ws";
        this.ws  = new WebSocket(this.url);
        try {
            // ws连接成功
            this.ws.onopen = function () {
                console.log("WebSocket Server [" + this.url +"] 连接成功！");
            };
            // ws连接关闭
            this.ws.onclose = function () {
                if (this.ws) {
                    this.ws.close();
                    this.ws = null;
                }
                console.log("WebSocket Server [" + this.url +"] 连接关闭！");
            };
            // ws连接错误
            this.ws.onerror = function () {
                if (this.ws) {
                    this.ws.close();
                    this.ws = null;
                }
                console.log("WebSocket Server [" + this.url +"] 连接关闭！");
            };
            // ws数据返回处理
            this.ws.onmessage = function (result) {
                console.log(" > " + result.data);
            };
        } catch (e) {
            alert(e.message);
        }
    },
    // 发送消息
    sendMessage() {
      // 
      if (this.ws == null) {
          console.log("WebSocket Server [" + this.url +"] 连接失败，请F5刷新页面!");
          return;
      }
      var content = this.message.replace("/[\n]/g", "");
      if (content.length <= 0) {
          console.log("请输入发送内容!");
          return;
      }
      this.message = "";

      console.log(content);
      this.ws.send(content);
    },
    // 处理消息
    handleMessage() {
      this.sendMessage()
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
</style>
