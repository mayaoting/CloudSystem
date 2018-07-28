<template>
    <div class="forget-password">
      <canvas id="canvas"  style="width:100%; height:100%;display: block; position: absolute;"></canvas>
      <div class="regist-head">
        <div class="regist-head-content">
          <img src="/static/image/云存储-01.png" width="200px" height="60px"/>
          <span><a href="#">找回密码</a></span>
          <div style="float: right;">
            <router-link to="/">登录</router-link>
            <router-link to="/regist">注册</router-link>
          </div>
        </div>
    </div>
      <div class="container-title" style="z-index: 999;position: relative">
        输入账号
      </div>
      <div class="container" style="z-index:999;position: relative">
        <form action="javascript:;">
          <div class="top-message-tip" style="height: 30px;margin: 50px auto 0 auto;">
            <p style="margin-bottom: 8px">请输入已注册的手机号/用户名</p>
          </div>
          <div class="container-form-account">
            <el-input
              placeholder="手机号/用户名"
              v-model="exitAccount"
              clearable
              name="phonenumber"
              @blur="isPhone"
              @focus="flag_exit_account=false"
              maxlength="11">
              <i slot="prefix" class="el-input__icon el-icon-erp-account"></i>
            </el-input>
          </div>
          <div class="input-warning">
            <p v-show="flag_exit_account" >手机号格式有误，请重新输入</p>
            <p v-show="flag2_exit_account" >用户名不能为空</p>
          </div>
          <div class="container-form-password">
            <el-input
              type="password"
              placeholder="密码请设置8-20个字符"
              v-model="newPassword"
              name="newpassword"
              clearable
              @focus="flag_new_password=false"
              @blur="isPassword"
              maxlength="20">
              <i slot="prefix" class="el-input__icon el-icon-erp-password"></i>
            </el-input>
          </div>
          <div class="input-warning">
            <p v-show="flag_new_password">密码不能少于8位字符，请重新输入</p>
          </div>
          <div class="container-form-password">
            <el-input
              type="password"
              placeholder="请再次确认密码"
              v-model="repeatPassword"
              clearable
              @focus="flag_repeat_password=false"
              @blur="isRepeatPassword"
              maxlength="20">
              <i slot="prefix" class="el-input__icon el-icon-erp-password"></i>
            </el-input>
          </div>
          <div class="input-warning">
            <p v-show="flag_validate_password">两次输入的密码不一致，请重新输入</p>
          </div>
          <div class="container-form-password">
            <el-input
              style="width: 200px;float: left;"
              placeholder="请输入短信验证码"
              name="message"
              v-model="checkMessageStr"
              clearable
              maxlength="6">
            </el-input>
            <div class="imgcheck" style="z-index: 999;position: relative">
              <el-button type="info" v-if="flag_getCode" class="getVerificationCode" @click="getCode">获取验证码</el-button>
              <el-button type="primary" v-if="!flag_getCode" class="getVerificationCode" >重新获取({{authTime}})</el-button>
            </div>
          </div>
          <div class="btn-submit" style="z-index: 999;position: relative">
            <el-button type="primary" style="z-index: 999; height: 40px;width: 100%" @click="click_confirm">确认</el-button>
          </div>
        </form>
      </div>
      <div class="other-info">
        <div style="border-right: 1px solid #828282">
          <span>帮助中心&#12288;</span>
        </div>
        <div>
          <span>&#12288;立即申诉</span>
        </div>
      </div>
      <div class="other-info" style="margin-top: 20px">
        <span>
          Copyright©2018-2018 360.CN All Rights Reserved 云存储系统<br/>
          <a href="#">京ICP证080047号[京ICP备08010314号-6]<br/>
          京公网安备 11000002000006号</a>
        </span>
      </div>
      <div style="height: 20px"></div>
      </div>
</template>

<script>
    export default {
        name: "forget-password",
      data(){
          return{
            exitAccount:'',
            flag_exit_account:false,
            flag2_exit_account:false,
            newPassword:'',
            flag_new_password:false,
            repeatPassword:'',
            flag_repeat_password:false,
            checkMessageStr:'',
            flag_validate_password:false,
            authTime:'60',
            flag_getCode:true,
            time:''
        }
      },
      methods:{
        isPhone(){
          let telreg = /^1[3,4,5,7,8,][0-9]{9}$/
          if (!telreg.test(this.exitAccount)){
            this.flag_exit_account = true;
          }
          if(this.exitAccount == null){
            this.flag2_exit_account = true;
          }
        },
        isPassword(){
          if (this.newPassword.length < 8 || this.newPassword.length > 20){
            this.flag_new_password = true;
          }
          else{
            this.flag_new_password = false;
          }
        },
        isRepeatPassword(){
          if(this.newPassword !== this.repeatPassword){
            this.flag_validate_password = true;
          }
        },
        click_confirm(){
          if (!this.exitAccount || !this.newPassword || !this.repeatPassword || !this.checkMessageStr){
            this.$message.error("请将信息填写完整!");
            return;
          }

          this.$axios.post('/auth/validateNum',
            {
              msgNum:this.checkMessageStr,
            })
            .then(function (res) {
              console.log(res);
              if(res.data.status == true){
                this.$message("重置密码成功，请返回登录");
                this.$router.push('/');
              }
            }.bind(this))
            .catch(function(error){
              console.log(error);
            })

        },
        getCode(){
          this.flag_getCode = false;
          this.authTime = 60;
          let auth_timer = setInterval(()=>{
            this.authTime--;
            if(this.authTime <= 0){
              this.flag_getCode = true;
              clearInterval(auth_timer);
            }
          },1000);

          this.$axios.post('/auth/loginByPhone',{
            phoneNumber:this.exitAccount,
            password:this.newPassword
          })
            .then(function (res) {

              console.log(res);
              // messagehash = res.data.hash;
              // tamp = res.data.tamp;
              // console.log(messagehash);
              // console.log(tamp);
            })
            .catch(function(error){
              console.log(error);
            })
        },


      },
      created(){

      },
      mounted(){
        class Circle {
          //创建对象
          //以一个圆为对象
          //设置随机的 x，y坐标，r半径，_mx，_my移动的距离
          //this.r是创建圆的半径，参数越大半径越大
          //this._mx,this._my是移动的距离，参数越大移动
          constructor(x, y) {
            this.x = x;
            this.y = y;
            this.r = Math.random() * 5 ;
            this._mx = Math.random() ;
            this._my = Math.random() ;
          }

          //canvas 画圆和画直线
          //画圆就是正常的用canvas画一个圆
          //画直线是两个圆连线，为了避免直线过多，给圆圈距离设置了一个值，距离很远的圆圈，就不做连线处理
          drawCircle(ctx) {
            ctx.beginPath();
            //arc() 方法使用一个中心点和半径，为一个画布的当前子路径添加一条弧。
            ctx.arc(this.x, this.y, this.r, 0, 2 * Math.PI);
            ctx.closePath();
            ctx.fillStyle = 'rgba(204, 204, 204, 0.3)';
            ctx.fill();
          }

          drawLine(ctx, _circle) {
            let dx = this.x - _circle.x;
            let dy = this.y - _circle.y;
            let d = Math.sqrt(dx * dx + dy * dy);
            if (d < 120) {
              ctx.beginPath();
              //开始一条路径，移动到位置 this.x,this.y。创建到达位置 _circle.x,_circle.y 的一条线：
              ctx.moveTo(this.x, this.y);   //起始点
              ctx.lineTo(_circle.x, _circle.y);   //终点
              ctx.closePath();
              ctx.strokeStyle = 'rgba(204, 204, 204, 0.3)';
              ctx.stroke();
            }
          }

          // 圆圈移动
          // 圆圈移动的距离必须在屏幕范围内
          move(w, h) {
            this._mx = (this.x < w && this.x > 0) ? this._mx : (-this._mx);
            this._my = (this.y < h && this.y > 0) ? this._my : (-this._my);
            this.x += this._mx / 2;
            this.y += this._my / 2;
          }
        }
        //鼠标点画圆闪烁变动
        class currentCirle extends Circle {
          constructor(x, y) {
            super(x, y)
          }

          drawCircle(ctx) {
            ctx.beginPath();
            //注释内容为鼠标焦点的地方圆圈半径变化
            //this.r = (this.r < 14 && this.r > 1) ? this.r + (Math.random() * 2 - 1) : 2;
            this.r = 1;
            ctx.arc(this.x, this.y, this.r, 0, 2 * Math.PI);
            ctx.closePath();
            //ctx.fillStyle = 'rgba(0,0,0,' + (parseInt(Math.random() * 100) / 100) + ')'
            ctx.fillStyle = 'rgba(255, 77, 54, 0.6)';
            ctx.fill();

          }
        }
        //更新页面用requestAnimationFrame替代setTimeout
        window.requestAnimationFrame = window.requestAnimationFrame || window.mozRequestAnimationFrame || window.webkitRequestAnimationFrame || window.msRequestAnimationFrame;

        let canvas = document.getElementById('canvas');
        let ctx = canvas.getContext('2d');
        let w = canvas.width = canvas.offsetWidth;
        let h = canvas.height = canvas.offsetHeight;
        let circles = [];
        let current_circle = new currentCirle(0, 0)

        let draw = function () {
          ctx.clearRect(0, 0, w, h);
          for (let i = 0; i < circles.length; i++) {
            circles[i].move(w, h);
            circles[i].drawCircle(ctx);
            for (let j = i + 1; j < circles.length; j++) {
              circles[i].drawLine(ctx, circles[j])
            }
          }
          if (current_circle.x) {
            current_circle.drawCircle(ctx);
            for (var k = 1; k < circles.length; k++) {
              current_circle.drawLine(ctx, circles[k])
            }
          }
          requestAnimationFrame(draw)
        }

        let init = function (num) {
          for (var i = 0; i < num; i++) {
            circles.push(new Circle(Math.random() * w, Math.random() * h));
          }
          draw();
        }
        window.addEventListener('load', init(100));
        window.onmousemove = function (e) {
          e = e || window.event;
          current_circle.x = e.clientX;
          current_circle.y = e.clientY;
        }
        window.onmouseout = function () {
          current_circle.x = null;
          current_circle.y = null;

        };
      }
    }
</script>

<style scoped>
  @import "../icon/iconfont.css";
  *{
    margin: 0;
    padding: 0;
  }
  p{
    margin-left: 22%;
  }
  .forget-password{
    background-color: rgb(245,245,245);
    min-width: 700px;
    height: 100%;
  }
  .regist-head{
    background-color: rgb(232,232,232);
    height: 68px;
  }
  .regist-head-content{
    width: 740px;
    min-width: 700px;
    margin: 0 auto;
    height: 68px;
    line-height: 68px;
  }
  .regist-head-content img{
    margin-right: 25px;
    margin-top: -10px;
  }
  .regist-head-content span{
    border-left: 1px solid #929292;
    color: #000;
    font-size: 20px;
    height: 30px;
    line-height: 30px;
    padding-left: 25px;
  }
  .regist-head-content a{
    text-decoration: none;
    color: black;
    margin-right: 20px;
  }
  .regist-head-content a:hover{
    color: deepskyblue;
  }
  .container-title{
    clear: both;
    width: 700px;
    min-width: 700px;
    margin: 72px auto 2px auto;
    background-color: white;
    height: 70px;
    font: 20px 'Microsoft YaHei';
    text-align: center;
    padding: 20px;
  }
  .container{
    width: 700px;
    min-width: 700px;
    margin: 0 auto;
    background-color: white;
    height: 500px;
    font-size: 20px;
    padding: 20px;
  }
  .container-form-account{
    width: 380px;
    height: 40px;
    margin:  0 auto;
  }
  .container-form-password{
    width: 380px;
    height: 40px;
    margin: 0 auto;
  }
  .input-warning{
    margin: 0 auto;
    height: 30px;
    line-height: 30px;
    font-size: 10px;
    color: red;
  }
  p{
    font-size: 14px;
  }
  .btn-submit{
    width: 380px;
    height: 50px;
    margin: 40px auto 0 auto;
  }
  span{
    color: #828282
  }
  .other-info{
    width: 700px;
    min-width: 700px;
    margin: 20px auto 0 auto;
    text-align: center;
  }
  .other-info span{
    font-size: 14px;
  }
  .other-info a{
    color: #828282;
    text-decoration: none;
    font-size: 14px;
  }
  .imgcheck{
    display: inline-block;
    /*margin-left: 10px;*/
    /*border:1px solid #000;*/
    /*height: 40px;*/
    float: right;
    /*cursor: pointer;*/
  }
  .getVerificationCode{
    padding: 10px;
    margin-right: 10px;
  }
  .other-info div{
    display: inline-block;
  }
</style>
