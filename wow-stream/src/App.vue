<template>
  <div id="app">
    <div class="page" id="user">
      <a href="http://www.tgbus.com/" class="top"></a>
      <div id="playground">
        <div class="room">
          <message
          :message="message"
          :listen.sync="listen"
          :keys.sync="keys"
          v-for="message of list"></message>
        </div>
      </div>
      <a href="http://app.nga.cn/phone.html" rel="noreferrer" target="_blank" class="console"></a>
    </div>
  </div>
</template>

<script>
import wx from 'wx'
import store from 'store'
import sdk from './api/sdk'
import Wilddog from 'Wilddog'
import Message from './components/Message'

export default {
  components: {
    Message
  },
  data () {
    return {
      listen: '',
      list: [],
      keys: [],
      msg: '正在加载中...',
      wechat: {
        title: 'TG兽吧直播室：魔兽影片主创邓肯·琼斯演员吴彦祖，葆拉·帕顿将与粉丝们见面！',
        link: 'http://lab.myriptide.com/wow/',
        imgUrl: 'http://lab.myriptide.com/wow/wechat.jpg',
        desc: '为您带来现场第一时间图文及音频直播。'
      }
    }
  },
  created () {
    const ref = new Wilddog('https://cast.wilddogio.com/voice/')
    sdk.get((err) => {
      if (err) {
        console.log(err)
      } else {
        wx.ready(() => {
          wx.onMenuShareTimeline(this.wechat)
          wx.onMenuShareAppMessage(this.wechat)
        })
        ref.on('child_added', (snapshot) => {
          let obj = snapshot.val()
          obj.key = snapshot.key()
          this.list.push(obj)
        })
        this.keys = store.get('keys') || []
        this.msg = '加载完成'
      }
    })
  },
  watch: {
    keys (val) {
      store.set('keys', this.keys)
    }
  }
}
</script>

<style>
* {
  margin: 0;
  padding: 0;
}
body, html {
  height: 100%;
  -webkit-tap-highlight-color: transparent;
  font-family:'Helvetica Neue', Helvetica, Microsoft Yahei;
}
#app {
  height: 100%;
}
.page, body {
  background-color: #fff;
}
.page {
  overflow-y: auto;
  -webkit-overflow-scrolling: touch;
  height: 100%;
  width: 100%;
  overflow-x: hidden;
}
.page-desc {
  text-align: center;
  font-size: 1rem;
  margin-top: 1rem;
  color: #888;
}
.spinner {
  width: 5rem;
  display: inline-block;
  text-align: center;
}
.spinner > i {
  width: .8rem;
  height: .8rem;
  margin-top: .1rem;
  background-color: rgba(60, 197, 31, 0.5);
  border-radius: 100%;
  display: inline-block;
  -webkit-animation: bouncedelay 1.4s infinite ease-in-out;
  animation: bouncedelay 1.4s infinite ease-in-out;
  /* Prevent first frame from flickering when animation starts */
  -webkit-animation-fill-mode: both;
  animation-fill-mode: both;
}
.spinner .bounce1 {
  -webkit-animation-delay: -0.32s;
  animation-delay: -0.32s;
}
.spinner .bounce2 {
  -webkit-animation-delay: -0.16s;
  animation-delay: -0.16s;
}
@keyframes bouncedelay {
  0%, 80%, 100% {
    transform: scale(0.0);
    -webkit-transform: scale(0.0);
  } 40% {
    transform: scale(1.0);
    -webkit-transform: scale(1.0);
  }
}
#user .page-title {
  position: absolute;
  left: 0;
  top: 0;
  width: 100%;
  z-index: 2;
}
#playground {
  padding-top: 60px;
  width: 100%;
  height: calc(100% - 130px);
  background: #efefef;
  overflow: auto;
}
.room {
  padding:.75rem;
}
.center {
  text-align: center;
}
.console {
  position: absolute;
  left: 0;
  bottom: 0;
  width: 100%;
  z-index: 2;
  height: 70px;
  background: url('http://lab.myriptide.com/wow/bottom.jpg') #212121 50% no-repeat;
  background-size: contain;
}
#user .top {
  position: absolute;
  left: 0;
  top: 0;
  width: 100%;
  z-index: 2;
}
.top {
  height: 60px;
  background: url('http://lab.myriptide.com/wow/top.jpg') #fff 50% no-repeat;
  background-size: 70%;
}
</style>
