<template>
  <div class="container" style="width: 800px; margin: auto; display: flex; flex-direction: column">
    <div style="text-align: center; margin-top: 100px">
      <div class="subtitle">小姐姐大舞台</div>
      <div id="mse"></div>
      <div class="buttons">
        <button @click="prev">上一个</button>
        <button @click="next">下一个</button>
        <button @click="download">下载</button>
      </div>
    </div>
  </div>
</template>

<script>
import Player from "xgplayer";
import { Events } from 'xgplayer'
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
            this.initPlayer(data.video)
            this.urlList.push(data.video);
          });
        }
      );
    },
    initPlayer(src) {
      const player = new Player({
        id: "mse",
        url: src,
        height: 450,
        width: 800,
        autoplay: true,
        autoplayMuted: true,
      });
      player.on(Events.ERROR, async () => {
        fetch("https://api.pearktrue.cn/api/random/xjj/?type=json").then(
          (res) => {
            res.json().then((data) => {
              player.src = data.video
            });
          }
        );
      })
      this.player = player;
    },
    next() {
      if (this.index < this.urlList.length - 1) {
        this.index++;
        this.player.src = this.urlList[this.index];
        return;
      }
      fetch("https://api.pearktrue.cn/api/random/xjj/?type=json").then(
        (res) => {
          res.json().then((data) => {
            this.player.src = data.video;
            this.urlList.push(data.video);
            this.index++
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
    download() {
      fetch(this.urlList[0])
        .then(res => res.blob())
        .then(blob => {
          const a = document.createElement("a");
          const objectUrl = window.URL.createObjectURL(blob);
          a.download = name;
          a.href = objectUrl;
          a.click();
          window.URL.revokeObjectURL(objectUrl);
          a.remove();
        })
    }
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
  font-weight: bold;
  font-size: 25px;
  margin-top: 10px;
}
</style>
