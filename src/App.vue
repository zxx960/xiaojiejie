<template>
  <div class="container" style="width: 800px; margin: auto">
    <div style="text-align: center">
      <div class="title">无聊小姐姐</div>
      <div class="title">无聊就来刷小姐姐吧</div>
      <vue3videoPlay v-bind="options" />
      <div class="buttons">
        <button @click="prev">上一个</button>
        <button @click="next">下一个</button>
      </div>
    </div>
  </div>
</template>

<script>
import "vue3-video-play/dist/style.css";
import vue3videoPlay from "vue3-video-play"; // 引入组件
export default {
  components: {
    vue3videoPlay,
  },
  data() {
    return {
      urlList: [],
      index: 0,
      options: {
        width: "800px", //播放器高度
        height: "450px", //播放器高度
        color: "#409eff", //主题色
        title: "", //视频名称
        src: " ", //视频源
        muted: false, //静音
        webFullScreen: false,
        speedRate: ["0.75", "1.0", "1.25", "1.5", "2.0"], //播放倍速
        autoPlay: true, //自动播放
        loop: false, //循环播放
        mirror: false, //镜像画面
        ligthOff: false, //关灯模式
        volume: 0.3, //默认音量大小
        control: true, //是否显示控制
        controlBtns: [
          "audioTrack",
          "quality",
          "speedRate",
          "volume",
          "setting",
          "pip",
          "pageFullScreen",
          "fullScreen",
        ], //显示所有按钮,
      },
    };
  },
  mounted() {
    this.getVideo();
  },
  methods: {
    getVideo() {
      fetch("https://api.pearktrue.cn/api/random/xjj/?type=json").then(
        (res) => {
          res.json().then((data) => {
            this.options.src = data.video;
            this.urlList.push(data.video);
          });
        }
      );
    },
    next() {
      if (this.index < this.urlList.length-1) {
        this.index++;
        this.options.src = this.urlList[this.index];
        return
      }
      fetch("https://api.pearktrue.cn/api/random/xjj/?type=json").then(
        (res) => {
          res.json().then((data) => {
            this.options.src = data.video;
            this.urlList.push(data.video);
            this.index = this.urlList.length-1
          });
        }
      );
    },
    prev() {
      if (this.index == 0) {
        alert("已经是第一集了");
      } else {
        this.index--;
        this.options.src = this.urlList[this.index];
      }
    },
  },
};
</script>
<style scoped>
.title {
  font-size: 30px;
  font-weight: bold;
  color: #000;
}
</style>
