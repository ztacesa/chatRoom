<template>
  <div class="chat-body">
    <div class="chat-header">{{msgreceiver}}</div>
    <div class="chat-display">
      <div v-for="msg in msglist" class="chat-box"
                                  :class="{
                                    'chat-box-me': msg.from === username,
                                    'chat-box-others': msg.from !== username
                                  }"
        >
        <img src="" class="user-img-base"
             :class="{
                'user-img-me': msg.from === username,
                'user-img-others': msg.from !== username
             }"
        >
        <div class="chat-bar-base"
             :class="{
                'chat-bar-me': msg.from === username,
                'chat-bar-others': msg.from !== username
             }"
        >{{msg.data}}</div>
      </div>
    </div>
    <div class="input-box">
      <textarea v-model.trim="msg" @keyup.enter="msgSubmit"></textarea>
      <button @click="msgSubmit">发送</button>
    </div>
  </div>
</template>

<style scoped>
.chat-body {
  position:relative;
  height:600px;
  background-color:rgb(238, 238, 238);
}
.chat-header {
  height:48px;
  line-height:48px;
  box-shadow: 0 1px 0 rgb(200, 200, 200);
  margin-bottom:2px;
  cursor:pointer;
}
.chat-display {
  height:400px;
  margin:0;
  overflow-y:auto;
  box-shadow:0 1px 0 rgb(200, 200, 200);
  padding:8px 0 8px 0;
}
.chat-box {
  padding:6px 10px 6px 10px;
  width: 705px;
}
.chat-box-me {
  text-align:right;
}
.chat-box-others {
  text-align:left;
}
.user-img-base {
  width:40px;
  height:40px;
  margin:0 5px 0 5px;
  vertical-align:top;
}
.user-img-me {
  float:right;
}
.user-img-others {
  float:left;
}
.chat-bar-base {
  display:inline-block;
  vertical-align:top;
  max-width:500px;
  min-height:40px;
  padding:10px 15px 10px 15px;
  border-radius:3px;
  word-wrap:break-word;
  word-break:break-all;
  text-align:left;
  font-size:14px;
  color:rgb(0, 0, 0);
}
.chat-bar-me {
  background-color:#b2e281;
}
.chat-bar-others {
  background-color:white;
}
.input-box {
  padding:8px 12px 8px 12px;
  height:150px;
}
.input-box textarea {
  outline:none;
  resize:none;
  border:none;
  width:100%;
  height:90px;
  overflow-y:hidden;
  background-color:rgb(238, 238, 238);
  font-size:16px;
  letter-spacing:1px;
}
.input-box button {
  font-size:12px;
  line-height:1;
  color:rgb(0, 0, 0);
  padding:8px 24px 8px 24px;
  border:none;
  box-shadow:0 0 1px #111111;
  border-radius:1px;
  background:white;
  outline:none;
  cursor:pointer;
}
</style>

<script>
import {mapState} from 'vuex'
export default {
  name: 'ChatInterface',
  data () {
    return {
      msg: ''
    }
  },
  computed: mapState(['username', 'friendlist', 'roomlist', 'msgreceiver', 'msglist']),
  methods: {
    msgSubmit () {
      if (this.msg === '') {
        return
      }
      this.$store.commit({
        type: 'setSelfmsg',
        selfmsg: {
          from: this.username,
          to: this.msgreceiver,
          data: this.msg
        }
      })
      this.$store.state.socket.emit('message', {
        from: this.username,
        to: this.msgreceiver,
        data: this.msg
      })
      this.msg = ''
    }
  }
}
</script>