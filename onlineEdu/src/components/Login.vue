<template>
   <div class='login'>
    <model-vue>
      <div slot='header'>
        <p class='x' @click='hide'>X</p>
        <span class='lf labelspan doregister' 
              :class='{active:isActive}' @click='doregister'>登陆</span>
        <span class=' labelspan dologin' 
            :class='{active:!isActive}' @click='doregister'>注册</span>
      </div>
      <div slot='body'>
        <div id='register-user'>
              <p>
                <span><i class="icon iconfont1">&#xe675;</i></span>
                <input type='text' placeholder='请输入账号' class='enter' v-focus v-model="username" />
              </p>
              <p>
                <span><i class="icon iconfont1">&#xe618;</i></span>
                <input type='password' placeholder='请输入密码' class='enter' v-model="password"/>
              </p>
              <div class=registerUser v-if='isActive'>
                <input type='checkbox'/>下次自动登陆
                <span class='rg'>忘记密码?</span>
              </div>
            </div>
      </div>
      <div slot='footer'>
        <button class='dobutton' v-if='isActive' @click='login'> 登陆</button>
        <button class='dobutton' v-else @click='regester'> 注册</button>
        <p class='registerUser1' v-if='isActive'>免费注册</p>
        <p class='registerUser1' v-else >去登陆</p>
      </div>
    </model-vue>
  </div>
</template>
<script>
  import Vue from 'vue'
  export default{
    data(){
      return{
        isActive: false,
        username:'',
        password:''
      }
    },
    methods:{
      hide(){
        this.$emit('click');
      },
      doregister(){
        this.isActive=!this.isActive;
      },
      login(){
        this.$http.post('/login',{
          username:this.username,
          password:this.password
        }).then((res)=>{
          console.log(res.data);
          //登录成功或者失败跳转的页面
        }).catch((error)=> {
          console.log(error);
        });
      
      },
      regester(){
        this.$http.post('/regester',{
          username:this.username,
          password:this.password
        }).then((res)=>{
          console.log(res.data);
          //注册成功或者失败
        }).catch((error)=> {
          console.log(error);
        });
      }
    },
    directives: {
      focus: {
        // 指令的定义
        inserted: function (el) {
          el.focus()
        }
      }
    },
    components:{
      'model-vue':{
        template:`
          <transition name="fade">
            <div class='model-bcak'>
              <div class='model'>
                <div class='model-header'>
                  <slot name='header'>
                    <h2>这是 Model 弹框的标题</h2>
                    <p class='btn-class' @click='hide'>X</p>
                  </slot>
                </div>
                <div class="model-body">
                  <slot name="body">
                    这是Modal弹框的主体
                  </slot>
                </div>
                <div class="model-footer">
                  <slot name="footer">
                    这是Modal弹框的脚部
                  </slot> 
                </div>
              </div>
            </div>   
          </transition>
        `,
      data(){
        return{}
      },
      methods:{
        hide(){
          this.$emit('click');
        }
      }
    }
  }
}
</script>
<style>
.model-bcak { 
  position: fixed;
  top: 0; right: 0; bottom: 0; left: 0; 
  background-color: rgba(0,0,0,.3); 
  display: flex;
  justify-content: center;
  align-items: center; 
  z-index: 2;
}
.model {
  padding: 20px;
 background: #fff;
}
.btn-class { 
  padding: 20px;
  margin-top: -20px;
  border: none;
  font-size: 20px;
  cursor: pointer; 
  font-weight: bold; 
  background: transparent;
}
.model-header, .model-footer,.model-body { 
  padding: 0 10px; 
  display: flex; 
 
}  
.fade-enter-active, .fade-leave-active {
  transition: opacity .5s;
}
.fade-enter, .fade-leave-to /* .fade-leave-active below version 2.1.8 */ {
  opacity: 0;
}

/*注册弹框的样式*/
p,input{margin: 0;padding:0;}
#regester{
  position: fixed;
  z-index: 9998;
  width: 100%;
  height: 100%;
  background-color: rgba(0, 0, 0, .7);
}

.page {
  width:300px;
  height: 350px;
  background: #fff;
  z-index: 9999;
  padding: 10px 20px;
  cursor: pointer;
}
.lf{  float: left;}
.rg{ float: right; }
@font-face {
  font-family: 'iconfont';  /* project id 568393 */
  src: url('//at.alicdn.com/t/font_568393_jsubtjnworox0f6r.eot');
  src: url('//at.alicdn.com/t/font_568393_jsubtjnworox0f6r.eot?#iefix') format('embedded-opentype'),
  url('//at.alicdn.com/t/font_568393_jsubtjnworox0f6r.woff') format('woff'),
  url('//at.alicdn.com/t/font_568393_jsubtjnworox0f6r.ttf') format('truetype'),
  url('//at.alicdn.com/t/font_568393_jsubtjnworox0f6r.svg#iconfont') format('svg');
}
 .iconfont1 {
  font-family:"iconfont" !important;
  font-size:25px;
  font-style:normal;
  -webkit-font-smoothing: antialiased;
  -webkit-text-stroke-width: 0.2px;
  -moz-osx-font-smoothing: grayscale;
}
.labelspan{
  width: 150px;
  line-height: 30px;
  font-size: 20px;
  justify-content: center;
  display: flex;
  color: #666;
  cursor: pointer;
}
#register-user p{
height: 40px;
border-radius: 5px;
border: 1px #eee solid;
margin: 20px 0;
}
#register-user p span{
  line-height: 37px;
  width: 40px;
  text-align: center;
  background: #eee;
  display: inline-block;

}
.x{
  width: 100%;
  height: 30px;
  font-size: 20px;
  color: #eee;
  text-align: right;
  cursor: pointer;
}
.enter{
  width: 240px;
  border: none;
  padding-left: 10px;
  line-height: 30px;
  font-size: 14px;
  outline: none;
}
.registerUser{ 
  font-size: 12px;
  padding: 15px;
}
.registerUser1{ 
  font-size: 12px;
  padding: 15px;
  text-align: right;
}
.dobutton{
  width: 300px;
  line-height: 38px;
  font-size: 18px;
  color: #fff;
  text-align: center;
  background: #00cc99;
  outline: none;
  border: none;
  margin-top: 10px ;
  cursor: pointer;
}
.active{ border-bottom: 2px #00cc99 solid; }
</style>