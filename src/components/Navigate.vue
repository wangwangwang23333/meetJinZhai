
<template>
    <el-header
    style="display: inline-block;width: 100%;margin-bottom: -5px;
    margin-top: -5px;
    position: absolute;left: 0;z-index: 10;">
      <el-menu 
      :default-active="activeIndex" 
      class="el-menu-demo" 
      mode="horizontal" 
      

      style="
      display: block;
      
      height: 100%;
      width: 111.8%;
      left: -10%;
      top:-5%;
      align-self: center;
      background-color: rgba(246,248,248,0.913);
      box-shadow: rgba(0, 0, 0, 0.12) 0px 1px 3px, rgba(0, 0, 0, 0.24) 0px 1px 2px;
      ">
        <el-menu-item style="width:20%;">
          <el-image 
            :src="require('@/assets/bigLogo.png')"
            style="width:60%;
            left: 25%;
            top:10%;
            "
          
            >
          </el-image>
        </el-menu-item>

        
        


      </el-menu>

    </el-header>
    
</template>
<link
  rel="stylesheet"
  href="https://cdnjs.cloudflare.com/ajax/libs/animate.css/4.1.1/animate.min.css"/>

<script>


export default {
  name: 'Navigate',
  components:{
  },
  created:function(){
    /*
    初始化，判断是否有token
    */
    

    //
  },
  mounted(){
    
  },
  methods:{
    changeName:function(newName){
      this.$refs.loginComponent.userName=newName;
    },
    //add@Lq
    routerToUserPage:function ()
    {
      console.log("进入个人")
      this.$router.replace('/userInfoPage');
    },
    // add@Lq
    routerToHostPage:function ()
    {
      console.log('hello')
      this.$router.replace('/hostInfoPage');
    },
    // add@ckx
    errorHandler(){
      return true
    },
  
    login(){
      this.dialogTableVisible=true;
      //更新验证码

    },
    closeLogin(){
      /*
      关闭登录窗口
      */
      console.log('登录窗口被关闭');
      this.dialogTableVisible=false;
    },
    isLegalPhone(){
        /*
        判断输入的手机号是否合法
        */
        var myreg = /^1[3|4|5|7|8|9][0-9]{9}$/;
        if (!myreg.test(this.$refs.loginComponent.phonenumber)) {
            console.log('手机号格式不正确')
            return false;
        } 
        else {
            console.log('手机号格式正确')
            return true;
        }
    },
    changeLoginState(){
      /*
      点击登录，检验信息
      */



      

      
    },
    register(){

      
      
    },
    getCurrentTime(){
        //获取当前时间字符串 如2021/7/4 10:58:38
        let yy=new Date().getFullYear();
        let mm=new Date().getMonth()+1;
        let dd=new Date().getDate();
        let hh=new Date().getHours();
        let mn=new Date().getMinutes()<10 ? '0'+new Date().getMinutes() : new Date().getMinutes();
        let ss=new Date().getSeconds()<10 ? '0'+new Date().getSeconds() : new Date().getSeconds();
        return yy+'/'+mm+'/'+dd+' '+hh+':'+mn+':'+ss;
    },
    setlocalHistory(val){
      /*
      点击搜索按钮后的逻辑处理
      @ckx
      */
      console.log('这里进来了')
      val=val.trim(); //去空格啥的
      val=this.getCurrentTime()+" "+val; //开头加上时间戳
      let localHistory=localStorage.getItem('localHistory');//获取历史记录
      if(localHistory===null){
          localStorage.setItem('localHistory',val)    // 若未设置过则直接设置本地存储
          console.log(localHistory);
      }else{
          let tempArray = localHistory.split('|')
          if(tempArray.length>0){
              localHistory = val + '|' + tempArray.join('|'); // 新增记录
          }
          if(localHistory.split('|').length>100){    //历史记录超过100项
              tempArray=localHistory.split('|');
              tempArray.pop(); //删除最旧的一项
              localHistory=tempArray.join('|');
          }
          localStorage.setItem('localHistory',localHistory);  //存入本地
      }
      console.log(localHistory);
    },
    //处理搜索栏跳转到相应的房源检索页面  written by wly
    handleSearchResult(){
      //通过传递两个参数，检索文本以及搜索类别.

      //点击搜索按钮后的逻辑
      if(this.searchText===''){
        this.$message({
          type:"info",
          message:"请输入搜索内容"
        })
        return;
      }
      
      //! 在这之后加入你的搜索与后端交互逻辑，就是点击搜索按钮后的各种跳转页面和传值
      let searchText=this.searchText;
      let searchValue=this.selectSearch;
      this.setlocalHistory(this.searchText);
      this.searchText='';
      
      this.$router.push({path:'/staysView',query:{searchText:searchText, searchValue:searchValue}}
      ).catch(err => {console.log('输出报错',err)});
      
    }
  },
  data(){
    return {
      selectSearch:'2',
      searchText:'',
      activeIndex:'1',
      loginState:0, //登录状态，先用这个
      dialogTableVisible: false,
      hasNewMessage:true,//是否有新消息
      getMessage:'',
      userName:'',//用户名
      userAvatar:'',//用户头像信息
      userIdentity:'',//用户身份信息
    }
  }
}
</script>



<style scoped>
.dialogClass{ 
  border-radius: 20px !important; 
}

.el-divider--vertical{
  display:inline-block;
  width:1px;
  height:70%;		
  margin:0 0 2 2px;
  vertical-align:middle;
  position:relative;
}

.login-dialog-box{
  background-image:'../assets/bg.jpg';
}

.login-dialog-box >>> .el-dialog {
  border-radius: 20px;
  box-shadow: rgba(50, 50, 93, 0.25) 0px 30px 60px -12px inset, rgba(0, 0, 0, 0.3) 0px 18px 36px -18px inset;

}
.login-dialog-box >>> .el-dialog .el-dialog__header{
  padding:0;
}
.login-dialog-box >>> .el-dialog .el-dialog__body {
  padding: 5px 10px 20px 20px;
}
.login-dialog-box >>> .el-dialog .el-dialog__footer {
  padding: 0px 10px 10px;
}
.loginButton{
  width: 100px;
  margin-right: 50px;
}
.registerButton{
  width: 100px;
}
.el-menu-item.is-active{
  font-weight: bold !important;
  background-color: rgba(0,0,0,0) !important;
}
.el-menu-item:hover{
  background-color: rgba(0,0,0,0) !important;
}
.el-menu:hover{
  background-color:none !important;
}

</style>

<style >
.el-submenu.el-submenu__title:hover{
  background-color:rgba(0,0,0,0) !important;
}
.el-submenu.el-submenu__title.is-active{
  background-color:rgba(0,0,0,0) !important;
}
</style>