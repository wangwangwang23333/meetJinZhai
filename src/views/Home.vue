<template>
  <div class="home">

    <el-container style="width: 100%;">
      <el-aside style="width: 70vw;">
        <video-player 
        class="video-player vjs-custom-skin" 
        ref="videoPlayer" 
        :playsinline="true" 
        :options="playerOptions">
        </video-player>
      </el-aside>
      <el-main >
        金寨县，隶属于安徽省六安市，位于皖西边陲、大别山腹地，地处三省七县二区结合部。西、南两面与河南省、湖北省毗邻，总面积3814平方千米。
金寨县是安徽省面积最大、人口最多的山区县和旅游资源大县，也是中国第二大将军县，被誉为“红军的摇篮、将军的故乡”，是革命老区。209、210省道纵贯南北，临近312国道；梅山水库、响洪甸水库可常年通航。
      </el-main>
    </el-container>

    
    
  

    <!--每日行程-->
    <el-card style="margin-top: 5vh;">
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
    <el-card style="margin-top: 5vh;">
      <div slot="header" class="clearfix">
        <span style="font-family:'FZWangDXCJW';font-size:26px;">重走金寨百年红色历史</span>
      </div>
      <el-timeline>
        <!--每一个节点的内容-->
        <div v-for="(redHistoryItem,index) in redHistoryData" :key="index">
        <el-timeline-item placement="top" :hide-timestamp="true">
          <el-card>
            <el-container>
              <el-header style="font-family:'FZZJ';font-size:22px;">{{redHistoryItem.name}}</el-header>
              <el-container>
                <!--每一个节点内事件内容-->
                <el-aside width="50%">
                  <el-card>
                    <el-collapse accordion>
                      <div v-for="(eventItem,index) in redHistoryItem.events" :key="index">
                      <el-collapse-item>
                        <template slot="title">
                          {{eventItem.eventName}}
                        </template>
                        <div style="font-family:FZFWZhu;font-size:18px;">{{eventItem.eventContent}}</div>
                      </el-collapse-item>
                      </div>
                    </el-collapse>
                  </el-card>
                </el-aside>

                <!--每一个节点内图片照片的内容-->
                <el-main>
                  <el-carousel :interval="3000" type="card" height="200px" indicator-position="none">
                    <div v-for="(ruinsItem,index) in redHistoryItem.ruins" :key="index">
                    <el-carousel-item v-for="(imageItem,imgIndex) in ruinsItem.ruinsImage" :key="imgIndex">
                      <el-card class="imageCard" style="height:250px;">
                        <el-image :src="imageItem" fit="fill" style="width:100%;height:150px;"></el-image>
                        <div style="font-family:FZFWZhu;font-size:18px;">{{ruinsItem.ruinsName}}</div>
                      </el-card>
                    </el-carousel-item>
                    </div>
                  </el-carousel>
                  
                </el-main>
              </el-container>
            </el-container>
            <el-image :src="require('@/assets/RedHistoryBGI.png')" style="position:absolute;right:0px;bottom:0px;opacity:0.4;"></el-image>
          </el-card>
          
        </el-timeline-item>
        </div>
      </el-timeline>
    </el-card>

    <!--唱一支山歌-->

    <!--调研成果-->
    <el-card style="margin-top: 5vh;">
      <div slot="header" class="clearfix">
        <span>调研成果</span>
      </div>
      这里写我们的问卷调查结果
    </el-card>

    <!--游戏-->
    <el-card style="margin-top: 5vh;">
      <div slot="header" class="clearfix">
        <span>游戏访金寨</span>
        <el-button style="float: right; padding: 3px 0" type="text"
        @click="changeGameState">
      <span v-if="gameStart">结束游戏</span>
      <span v-else>开始游戏</span>
      </el-button>
      </div>
      <el-container>
        <el-main>
          <game v-if="gameStart"></game>
        </el-main>
        <el-aside>
          我们制作了一款[吃金币]的游戏，在游戏的过程中你能够领略到金寨
          各地的美景。<br>随着吃金币到达所要求的的数量，你就能进入金寨
          的下一个场景，体会另一种风光。
          <br>
          【操作方式】
          <br>
          A 向左移动 D 向右移动
        </el-aside>
      </el-container>
    </el-card>
  </div>
</template>

<script>
import {videoPlayer} from "vue-video-player";
import "video.js/dist/video-js.css";
import redData from "@/assets/json/RedHistory.json"
import game from '@/components/game.vue';

export default {
  name: 'Home',
  components: {
    videoPlayer,
    game
  },
  data(){
    const self=this;
    return{
      gameStart:false,
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

      //red history
      redHistoryData:[],
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
    //redHistory
    this.redHistoryData=redData;
  },
  methods:{
    changeGameState(){
      this.gameStart=!this.gameStart;
    },
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
@import "../assets/css/font.css";
.el-collapse>>>.el-collapse-item__header{
  font-family: 'FZHeiBJW';
  font-size: 18px;
}
.imageCard>>>.el-card__body{
  padding:0px;
}
</style>

<style>
.el-message{
  min-width: 500px !important;
  background-color: white !important;
  background-image:require('@/assets/mountain.png') !important;
}

</style>