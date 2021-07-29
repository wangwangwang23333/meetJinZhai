<template>
  <div class="home">
    <el-container>
      <el-main>
        <el-image
        src="https://z3.ax1x.com/2021/07/29/WqgS54.png"
        >

        </el-image>
      </el-main>
      <el-aside>
        <h1 style="margin-top: 40%;">网页施工中，过几天再来看吧</h1>
      </el-aside>
    </el-container>
    <!--视频-->
    <div style="width: 20%;">
      这里放一个宣传片视频
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
      <el-container>
        <el-main>
          <el-amap
        :vid="'amap-vue'"
        :zoom="zoom"
        :center="center"
        style="height: 500px;"
        mapStyle="fresh"
        >
        <!--连线-->
        <el-amap-polyline 
        :editable="polyline.editable"  
        :path="polyline.path"
        :strokeOpacity="polyline.strokeOpacity" 
        :strokeStyle="polyline.strokeStyle"
        :lineJoin="polyline.lineJoin"
        :events="polyline.events">
      </el-amap-polyline>

        <div v-for="(marker,index) in markerGroups" :key="marker.index">
          <el-amap-marker 
          :extData="index" 
          :position="marker.location" 
          :label="marker.label" 
          :icon="require('@/assets/location.png')" 
          :events="markerEvents"
          >
          </el-amap-marker>
          <!--点击出现关于该地点的介绍-->
          <el-amap-info-window 
          v-if="marker.window" 
          :position="marker.location" 
          closeWhenClickMap ="true" 
          autoMove ="true">
          <!--走马灯展示照片-->

          {{marker.description}}
          </el-amap-info-window>
         </div>

        </el-amap>
        </el-main>
        <el-aside>
          旅行日志
          <el-button v-for="index in 6" :key="index"
          @click="readDiary(index)">
            8月{{index}}日
          </el-button>
        </el-aside>

      </el-container>
      <div>
        <!--map-->
        
        

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
    const self=this;
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

      center: [115.93463, 31.451],
      zoom:10,
      tripDescription:'正在加载，请稍后',
      //绘制折线
      polyline:{
        path:[],
        events:{

        },
        editable:false,
        strokeOpacity:0.5,
        strokeStyle:'dashed',
        lineJoin:'round',
      },
      //每天的信息
      markerGroups:[
        {
          index:1,
          window:false,
          label:{
            content:'8.1下午',
            offset:[2,47],
            direction:'right'
          },
          photo:[

          ],
          description:'8月1日下午抵达金寨火车站，旅途开始啦',
          location:[115.97567786190794,31.625587993304553]
        },
        {
          index:2,
          window:false,
          label:{
            content:'8.1晚上',
            offset:[2,47],
            direction:'right'
          },
          description:'晚上来到了红军广场，瞻仰了烈士纪念馆',
          location:[115.87467659924314,31.685132557074333]
        },
      ],

      //点击后的事件
      markerEvents:{
        click(e){
          let curMarker=e.target;
          let MarkerID=curMarker.Ce.extData;
          console.log(self.markerGroups[MarkerID].window)
          if(self.markerGroups[MarkerID].window){
            self.markerGroups[MarkerID].window=false;
          }
          else{
            //显示窗体
            self.markerGroups[MarkerID].window=true;
          }

        }
      }
    }
  },
  created(){
    this.$message({
      message: '欢迎来到「遇见金寨」',
      type: 'success'
    });

    for(let i=0;i<this.markerGroups.length;++i){
      this.polyline.path.push(this.markerGroups[i].location);
    }
  },
  methods:{
    readDiary(date){
      console.log('开始阅读日志')
      if(date==1){
        this.$message({
          message: '8月1日的日志还没有出炉哦!',
          type: 'info'
        });
      }
      else{
        this.$message({
          message: '8月'+date+'日的日志还没有出炉哦!',
          type: 'info'
        });
      }
    }
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