<template>
  <div class="home">

    <el-container>
      <el-aside>
        <video-player class="video-player vjs-custom-skin" ref="videoPlayer" :playsinline="true" :options="playerOptions">
        </video-player>
      </el-aside>
      <el-main>

      </el-main>
    </el-container>

    <!--视频-->
    <div style="width:50%;margin-left: 25%;">
      这里放一个宣传片视频
      
    </div>
    <div>
      你好
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
        <span>旅行路线</span>
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
          <el-carousel 
          indicator-position="outside" 
          v-if="marker.photo.length!=0"
          style="width: 400px;"
          height="200px"
          >
            <el-carousel-item v-for="item in marker.photo" :key="item">
              <el-image
              :src="item"
              >
              </el-image>
            </el-carousel-item>
          </el-carousel>

          <div v-else>
            这里暂时没有图片哦
          </div>

          <!--分割线-->
          <el-divider></el-divider>

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

    <!--重走长征路-->
    <el-card>
      <div slot="header" class="clearfix">
        <span>重走长征路</span>
      </div>
      <el-timeline>
        <el-timeline-item timestamp="2018/4/12" placement="top">
          <el-card>
            <h4>更新 Github 模板</h4>
            <p>王小虎 提交于 2018/4/12 20:46</p>
          </el-card>
        </el-timeline-item>
        <el-timeline-item timestamp="2018/4/3" placement="top">
          <el-card>
            <h4>更新 Github 模板</h4>
            <p>王小虎 提交于 2018/4/3 20:46</p>
          </el-card>
        </el-timeline-item>
        <el-timeline-item timestamp="2018/4/2" placement="top">
          <el-card>
            <h4>更新 Github 模板</h4>
            <p>王小虎 提交于 2018/4/2 20:46</p>
          </el-card>
        </el-timeline-item>
      </el-timeline>
    </el-card>

    <!--唱一支山歌-->

    <!--调研成果-->
    <el-card>
      <div slot="header" class="clearfix">
        <span>调研成果</span>
      </div>
      这里写我们的问卷调查结果
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
        playbackRates: [0.5,1.0,  2.0], //播放速度
        autoplay: true, //如果true,浏览器准备好时开始回放。
        muted: false, // 默认情况下将会消除任何音频。
        loop: true, // 导致视频一结束就重新开始。
        preload: "auto", // 建议浏览器在<video>加载元素后是否应该开始下载视频数据。auto浏览器选择最佳行为,立即开始加载视频（如果浏览器支持）
        language: "zh-CN",
        aspectRatio: "16:9", // 将播放器置于流畅模式，并在计算播放器的动态大小时使用该值。值应该代表一个比例 - 用冒号分隔的两个数字（例如"16:9"或"4:3"）
        fluid: true, // 当true时，Video.js player将拥有流体大小。换句话说，它将按比例缩放以适应其容器。
        sources: [
          {
            type: "video/mp4",
            src: require("../assets/videos/meetJinzhai.mp4"), //视频url地址
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
            content:'金寨火车站',
            offset:[2,47],
            direction:'right'
          },
          photo:[
            require("../assets/trival/1-1.png"),
            require("../assets/trival/1-2.png")
          ],
          description:'金寨县高铁站，可以选择直接乘坐高铁到【金寨站】，开始你的金寨之旅',
          location:[115.97567786190794,31.625587993304553]
        },
        {
          index:2,
          window:false,
          label:{
            content:'红军广场',
            offset:[2,47],
            direction:'center'
          },
          photo:[
            require("../assets/trival/2-1.png"),
            require("../assets/trival/2-2.png"),
          ],
          description:'红色旅途第一站———【红军广场】。',
          location:[115.87467659924314,31.685132557074333]
        },
        {
          index:3,
          window:false,
          label:{
            content:'梅山水库',
            offset:[2,47],
            direction:'center'
          },
          photo:[
            require("../assets/trival/3-1.png"),
            require("../assets/trival/3-2.png"),
          ],
          description:'梅山水库。',
          location:[115.89025486920164,31.660259990005017]
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