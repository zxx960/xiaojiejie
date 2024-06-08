<template>
  <div class="container" style="width: 100%;max-width: 800px; margin:0 auto; display: flex; flex-direction: column">
    <div style="text-align: center;margin-top: 50px;  ">
      <div class="subtitle">小姐姐大舞台</div>
      <div id="mse" style="margin-top: 20px;"></div>
      <div class="buttons" style="margin-top: 20px;">
        <van-button type="primary" size="small" @click="download">下载</van-button>
        <van-button type="primary" style="margin-left: 10px;" size="small" @click="prev">上一个</van-button>
        <van-button style="margin-left: 10px;" type="primary" size="small" @click="next">下一个</van-button>
      </div>
    </div>
  </div>
</template>

<script>
import Player from "xgplayer";
import { Events } from 'xgplayer'
import fileDownload from 'js-file-download'
import "xgplayer/dist/index.min.css";
import { Button } from 'vant';
export default {
  components: {
    [Button.name]: Button,
  },
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
        width: '100%',
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
      fetch(document.querySelector('video').src)
        .then(res => res.blob())
        .then(blob => {
          fileDownload(blob, `${Math.floor(Math.random() * 100000) + 1}.mp4`)
        })
    }
  },
};
</script>
<style scoped>
.subtitle {
  font-weight: bold;
  font-size: 25px;
  /* margin-top: 10px; */
}
</style>
