<template>
  <div
    class="container"
    style="width: 800px; margin: auto; display: flex; flex-direction: column"
  >
    <div style="text-align: center; margin-top: 100px">
      <div class="title">无聊小姐姐</div>
      <div class="subtitle">无聊就来刷小姐姐吧</div>
      <video
        :src="options.src"
        style="margin-top: 30px"
        width="800"
        height="450"
        controls
        autoplay
        muted
        webkit-playsinline="true"
        playsinline='true'
      ></video>
      <div class="buttons">
        <button @click="prev">上一个</button>
        <button @click="next">下一个</button>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  components: {},
  data() {
    return {
      urlList: [],
      index: 0,
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
            this.options.src = data.video;
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
            this.options.src = data.video;
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
        this.options.src = this.urlList[this.index];
      }
    },
    error(val) {
      console.log(val);
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
