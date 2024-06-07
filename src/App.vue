<template>
  <div
    class="container"
    style="width: 800px; margin: auto; display: flex; flex-direction: column"
  >
    <div style="text-align: center; margin-top: 100px">
      <div class="title">无聊小姐姐</div>
      <div class="subtitle">无聊就来刷小姐姐吧</div>
      <div id="mse"></div>
      <div class="buttons">
        <button @click="prev">上一个</button>
        <button @click="next">下一个</button>
      </div>
    </div>
  </div>
</template>

<script>
import Player from "xgplayer";
import "xgplayer/dist/index.min.css";
export default {
  components: {},
  data() {
    return {
      urlList: [],
      index: 0,
      player: null,
      options: {
        src: "", //视频源
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
            const player = new Player({
              id: "mse",
              url: data.video,
              height: 450,
              width: 800,
              autoplay: true,
              autoplayMuted: true,
            });
            this.player = player;
            this.urlList.push(data.video);
          });
        }
      );
    },
    next() {
      if (this.index < this.urlList.length - 1) {
        this.index++;
        this.options.src = this.urlList[this.index];
        return;
      }
      fetch("https://api.pearktrue.cn/api/random/xjj/?type=json").then(
        (res) => {
          res.json().then((data) => {
            this.player.src = data.video;
            this.urlList.push(data.video);
            this.index = this.urlList.length - 1;
          });
        }
      );
    },
    prev() {
      if (this.index == 0) {
        alert("已经是第一集了");
      } else {
        this.index--;
        this.player.src = this.urlList[this.index];
      }
    },
  },
};
</script>
<style scoped>
.title {
  font-size: 50px;
  font-weight: bold;
  color: #000;
}
.subtitle {
  font-size: 25px;
  margin-top: 10px;
}
</style>
