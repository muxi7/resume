<template>
  <div id="topbar">
    <div class="wrapper">
      <span class="logo">Resumer</span>
      <div class="actions">
        <div v-if="logined" class="userActions">
          <span class="welcome">你好,{{user.username}}</span>
          <a class="button" href="#" @click.prevent="signOut">退出</a>
        </div>
        <div v-else class="userActions">
          <a class="button primary" href="#" @click.prevent="signUpDialogVisible = true">注册</a>
          <MyDialog title="注册" :visible="signUpDialogVisible" @close="signUpDialogVisible = false">
            <SignUpForm @success="signIn($event)"/>
          </MyDialog>

          <a class="button" href="#" @click.prevent="signInDialogVisible=true">登录</a>
          <MyDialog title="登录" :visible="signInDialogVisible" @close="signInDialogVisible=false">
            <SignInForm @success="signIn($event)"/>
          </MyDialog>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import MyDialog from './MyDialog'
import SignUpForm from './SignUpForm'
import AV from '../lib/leancloud'
import SignInForm from './SignInForm'

export default{
  name:'Topbar',
  data(){
    return {
      signUpDialogVisible: false,
      signInDialogVisible: false
    }
  },
  computed:{
    user(){
      return this.$store.state.user
    },
    logined(){
      return this.user.id
    }
  },
  components:{
    MyDialog,
    SignUpForm,
    SignInForm
  },
  methods:{
    signIn(user){
      this.signUpDialogVisible = false
      this.signInDialogVisible = false
      this.$store.commit('setUser',user)
    },
    signOut(){
      AV.User.logOut()
      this.$store.commit('removeUser')
    }
  }
}
</script>

<style scoped lang="scss">
  #topbar{
  	font-size:20px;
  	background-color:#ffffff;
    box-shadow:0 1px 3px 0 rgba(0,0,0,0.25);
    
    >.wrapper{
      min-width:1024px;
      max-width:1440px;
      margin:0 auto;
      height:64px;
    }
    >.wrapper{
      display:flex;
      justify-content:space-between;
      align-items:center;
      padding:0 16px;
    }
    .logo{
      font-size:24px;
      color:#000000
    }
  }
  .button{
    width:72px;
    height:32px;
    margin:5px;
    border:none;
    cursor:pointer;
    font-size:18px;
    background-color:#ddd;
    color:#222;
    border-radius:5px;
    text-decoration:none;
    display:inline-flex;
    justify-content:center;
    align-items:center;
    vertical-align:middle;
    &:hover{
      box-shadow:1px 1px 1px hsla(0,0,0,0.5);
      background-color:#70edaa;
    }
    &:primary{
      background-color:#02af5f;
      color:white;
    }
  }
  .actions{
    display:flex;
    .userActions{
      .welcome{
        margin-right:.5em;
      }
    }
  }
  
</style>