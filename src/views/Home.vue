<template>
  <div class="home" >

    <el-card style="background:rgba(255,255,255,0.5);
    ">
      <el-container style="width: 100%;margin-bottom: 5vh;">
        <el-aside style="width: 60vw;">
          <video-player 
          class="video-player vjs-custom-skin" 
          ref="videoPlayer" 
          :playsinline="true" 
          :options="playerOptions">
          </video-player>
        </el-aside>
        <el-main style="font-family:FZFWZhu;font-size:20px;text-align: left;">
          <b style="font-family:'FZWangDXCJW';font-size:25px">&emsp;
            巍巍大别山，映照着英雄儿女的烈火青春</b>
          <br><br>
          &emsp;&emsp;        金寨县，隶属于安徽省六安市，位于皖西边陲、大别山腹地，地处三省七县二区结合部。西、南两面与河南省、湖北省毗邻，总面积3814平方千米。
  金寨县是安徽省面积最大、人口最多的山区县和旅游资源大县，也是中国第二大将军县，被誉为“红军的摇篮、将军的故乡”，是革命老区。209、210省道纵贯南北，临近312国道；梅山水库、响洪甸水库可常年通航。
        <br>
        &emsp;
        &emsp;
        在这块土地上，先后走出了59位开国将军、600多位共和国的高级将领，革命薪火在这里生生不息。习近平总书记在2016年4月24日亲临金寨视察，不仅对金寨老区人民为中国革命和建设做出的巨大牺牲和贡献给予了充分肯定和高度赞扬，并对红色文化资源的开发利用、红色基因的传承弘扬做出了重要指示。
  
        </el-main>
        <el-image :src="require('@/assets/RedHistoryBGI.png')" style="position:absolute;right:0.5vw;bottom:3.8vh;opacity:0.4;"></el-image>
      </el-container>
    </el-card>

    <!--每日行程-->
    <el-card style="margin-top: 5vh;">
      <div slot="header" class="clearfix" style="font-family:'FZWangDXCJW';font-size:26px;">
        <span>旅行路线</span>
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

        <div v-for="(marker,index) in markerGroups" :key="marker.index" style="font-family:FZFWZhu;font-size:18px;">
          <el-amap-marker 
          :extData="index" 
          :position="marker.location" 
          :label="marker.label" 
          :icon="require('@/assets/location.png')" 
          :events="markerEvents"
          style="font-family:FZFWZhu;font-size:18px;"
          >
          </el-amap-marker>
          <!--点击出现关于该地点的介绍-->
          <el-amap-info-window 
          v-if="marker.window" 
          :position="marker.location" 
          closeWhenClickMap ="true" 
          style="font-family:FZFWZhu;font-size:18px;"
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
          <p style="font-family:FZFWZhu;font-size:18px;">{{marker.description}}</p>
          
          </el-amap-info-window>
         </div>

        </el-amap>
        </el-main>
        <el-aside style="font-family:FZFWZhu;font-size:20px;width: 30vw;">
          <p style="margin-top: 10%;text-align: left;">
            我们在这里提供了一条比较好的金寨旅游路线。沿着我们的路线，就可以一览金寨的红色遗迹，体会那段红色岁月。
            <br>
            从抵达【<b>金寨火车站</b>】的那一刻开始，我们的旅途就开始了。我们先到达了【<b>红军广场</b>】，看着高耸的革命烈士纪念塔，瞻仰着红军纪念堂。其后，我们又来到了【<b>梅山水库</b>】，感受着具有综合效益的大型水利水电工程切实发挥的作用。告别了梅山水库，我们前往【<b>天堂寨</b>】。天堂寨古称衡山，又名多云山，是大别山脉第二高峰。它雄踞于皖鄂大别山主峰接壤处，自古为兵家必争之处，帝王巡幸之所，名人登临之境。
            <br><br>
            然而，我们的旅途还有很远很远……
          </p>
        </el-aside>
        <el-image :src="require('@/assets/RedHistoryBGI.png')" style="position:absolute;right:0.5vw;bottom:-93vh;opacity:0.4;"></el-image>
      </el-container>
      

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

    

    <!--游戏-->
    <el-card style="margin-top: 5vh;padding-bottom: 10px !important;height: 100vh !important;">
      <div slot="header" class="clearfix" style="font-family:'FZWangDXCJW';font-size:26px;">
        <span>游戏访金寨</span>
        <el-button style="float: right; padding: 3px 0" type="text"
        @click="changeGameState">
      <span v-if="gameStart" style="font-family:FZFWZhu;font-size:18px;">结束游戏</span>
      <span v-else style="font-family:FZFWZhu;font-size:18px;">开始游戏</span>
      </el-button>
      </div>
      <el-container>
        <el-main>
          <game v-if="gameStart"></game>
          <el-image v-else :src="require('@/assets/gameWait.png')"></el-image>
        </el-main>
        <el-aside style="font-family:FZFWZhu;font-size:19px;text-align: left;">
          <p style="margin-top: 10%;">
            我们制作了一款[<b>吃金币</b>]的游戏，在游戏的过程中你能够领略到金寨各地的美景，聆听着悦耳的山歌《八月桂花遍地开》。<br><br>随着吃金币到达所要求的的数量，你就能进入金寨
          的下一个场景，体会另一种风光。
          <br><br>
          【操作方式】
          <br>
          A 向左移动 D 向右移动
          <br><br>
          【游戏说明】
          <br>
          ○&emsp;吃金币，获得积分
          <br>
          ○&emsp;获得足够的积分以通关
          <br>
          ○&emsp;旅途中体会金寨美景
          <br>
          ○&emsp;倾听悦耳的山歌
          <br><br>
          我们在金寨，等待你的到来

          </p>
        </el-aside>
        
      </el-container>
      <el-image :src="require('@/assets/RedHistoryBGI.png')" style="right:-40vw;bottom:20vh;opacity:0.4;"></el-image>
      
    </el-card>

    <!--调研成果-->
    <el-card style="margin-top: 5vh;">
      <div slot="header" class="clearfix" style="font-family:'FZWangDXCJW';font-size:26px;">
        <span>调研成果</span>
      </div>
      <el-image :src="require('@/assets/survey.png')"
      style="width: 80%;"></el-image>
    </el-card>

    <el-image :src="require('@/assets/bigLogo.png')" style="width: 15%;margin-top: 5%"></el-image>
    <br>
    <p style="font-family:FZFWZhu;font-size:18px;">等你一起探索……</p>
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
          description:'梅山水库位于鄂、豫、皖三省交界处的大别山腹地、淮河支流史河上游，坐落于有“红军故乡、将军摇篮”之誉的安徽省金寨县县城南端。它位于史河上游，坝址在安徽省金寨县梅山镇大小梅山之间。北距史河入淮口130km，是一座以防洪、灌溉为主，兼有发电等综合效益的大型水利水电工程。',
          location:[115.89025486920164,31.660259990005017]
        },
        {
          index:4,
          window:false,
          label:{
            content:'悬剑山风景区',
            offset:[2,47],
            direction:'center'
          },
          photo:[
            require("../assets/trival/6-1.png"),
            require("../assets/trival/6-2.png"),
          ],
          description:'清人陈自卿聚义士起义，置兵书、挂宝剑于峰顶，以示天意，故尔得名。悬剑山覆盖面积25平方公里，有景点50余处，其中待命名30余处，植被完整，物种多样，具有丰富的旅游资源。2000年，县宗教局、旅游局批准，悬剑山为宗教旅游区。',
          location:[115.71805832641598,31.599636909852173]
        },
        {
          index:5,
          window:false,
          label:{
            content:'天堂寨',
            offset:[2,47],
            direction:'center'
          },
          photo:[
            require("../assets/trival/4-1.png"),
            require("../assets/trival/4-2.png"),
            require("../assets/trival/4-3.png"),
          ],
          description:'天堂寨是大别山第二高峰，由十一届全国人大常委会委员长吴邦国先生题字。位于安徽省金寨县与湖北省罗田县交界的地区，有“华东最后一片原始森林、植物的王国、花的海洋”的美称。',
          location:[115.806150,31.199840]
        },
        {
          index:6,
          window:false,
          label:{
            content:'燕子河大峡谷',
            offset:[2,47],
            direction:'center'
          },
          photo:[
            require("../assets/trival/5-1.png"),
            require("../assets/trival/5-2.png"),
          ],
          description:'燕子河大峡谷位于安徽省六安市金寨县境内，由著名书画家启功先生题字。与华东最后一片原始森林天堂寨毗邻，峡谷奇峡绵延，以险崖、奇石、幽谷、秀水而著称',
          location:[115.89589951293942,31.244699681850488]
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
      date;
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