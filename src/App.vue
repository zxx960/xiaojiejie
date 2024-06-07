<template>
  <div class="container" style="width: 800px;margin: auto;">
    <div style="text-align: center;">
      <div class="title">无聊小姐姐</div>
      <div class="title">无聊就来刷小姐姐吧</div>
      <video :src="url" controls width="300"></video>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      url: ''
    };
  },
   mounted() {
    fetch('https://api.pearktrue.cn/api/random/xjj/?type=json').then(res => {
      res.json().then(data => {
        this.url = data.video;
      })
    })
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
