<template>
  <div class="home">
    begin to work
    <!--视频-->
    <div style="width: 20%;">
      <video-player class="video-player vjs-custom-skin" ref="videoPlayer" :playsinline="true" :options="playerOptions">
      </video-player>
    </div>
    <!--金寨地图-->
    <div style="width: 50%;height: 10%;">
      金寨：
      <el-amap vid="1" >
      </el-amap>
    </div>

    <!--每日行程-->
    <el-card>
      <div slot="header" class="clearfix">
        <span>每日行程</span>
        <el-button style="float: right; padding: 3px 0" type="text">操作按钮</el-button>
      </div>
      <div>
        <!--map-->

        我是内容
      </div>
    </el-card>

  </div>
</template>

<script>
import {videoPlayer} from "vue-video-player";
import "video.js/dist/video-js.css";

export default {
  name: 'Home',
  components: {
    videoPlayer
  },
  data(){
    return{
      playerOptions: {
        playbackRates: [0.7, 1.0, 1.5, 2.0], //播放速度
        autoplay: false, //如果true,浏览器准备好时开始回放。
        muted: false, // 默认情况下将会消除任何音频。
        loop: false, // 导致视频一结束就重新开始。
        preload: "auto", // 建议浏览器在<video>加载元素后是否应该开始下载视频数据。auto浏览器选择最佳行为,立即开始加载视频（如果浏览器支持）
        language: "zh-CN",
        aspectRatio: "16:9", // 将播放器置于流畅模式，并在计算播放器的动态大小时使用该值。值应该代表一个比例 - 用冒号分隔的两个数字（例如"16:9"或"4:3"）
        fluid: true, // 当true时，Video.js player将拥有流体大小。换句话说，它将按比例缩放以适应其容器。
        sources: [
          {
            type: "video/mp4",
            src: require("../assets/videos/meetYou.mp4"), //视频url地址
          },
        ],
        poster: require("../assets/videos/meetYou.jpg"), //你的封面地址
        // width: document.documentElement.clientWidth,
        notSupportedMessage: "此视频暂无法播放，请稍后再试", //允许覆盖Video.js无法播放媒体源时显示的默认信息。
        controlBar: {
          timeDivider: true,
          durationDisplay: true,
          remainingTimeDisplay: false,
          fullscreenToggle: true, //全屏按钮
        },
      },

    


    }
  },
  created(){
    this.$message({
      message: '欢迎来到「遇见金寨」',
      type: 'success'
    });
  }
}
</script>

<style scoped>

</style>

<style>
.el-message{
  min-width: 500px !important;
  background-color: white !important;
  background-image:require('@/assets/mountain.png') !important;
}

</style>