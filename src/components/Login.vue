//通过类选择器加样式
//图标使用了第三方的图标库 这里使用了阿里的图标库
<!--数据双向绑定 :el-form 通过属性绑定一个model属性，值是一个数据对象，这个值在 date中进行定义，接下来用每个具体的表单项 进行v-model的双向绑定，绑定到model中的具体属性中为表单项添加先在model绑定-->
<!--数据验证 1.el-form 通过属性绑定指定 rules 校验对象，2.在data中定义校验对象其中每一个属性都是验证规则，3.为不同的表单item项通过prop指定验证规则-->
<!--重置resetFields对整个表单进行重置，将所有字段值重置为初始值并移除校验结果,首先为表单添加ref引用,这个值是表单的实例对象，用this访问ref 是引用对象,再调用resetfields方法 -->
<!--表单数据的预验证 调用validate可以对整个表单进行校验，用ref来调用这个函数-第一个参数是布尔值判断是不是合法 -->
<!--路由导航守卫控制访问权限 通过用户访问的地址以及是否有tokenstr来确定是否是对的-->
<template>
 <div class="login_container">
   <div class="loin_box">
        <!-- 头像区域-->
     <div class="avatar_box">
       <img src="../assets/logo.png" alt="">
     </div>
         <!--登陆表单区-->
     <el-form ref="loginFormRef" :model="loginForm" :rules="loginFormRules" label-width="0px" class="login_form">
        <!-- 用户名字-->
       <el-form-item prop="username">
         <el-input v-model="loginForm.username"  prefix-icon="iconfont icon-user"> </el-input>
       </el-form-item>
       <!-- 密码-->
       <el-form-item prop="password">
         <el-input v-model="loginForm.password" prefix-icon="iconfont icon-3702mima" type="password"> </el-input>
       </el-form-item>
       <!-- 按钮区域-->
       <el-form-item class="btns">
         <el-button type="primary" @click="login">登录</el-button>
         <el-button type="info" @click="resetLoginForm">重置</el-button>
       </el-form-item>
     </el-form>
   </div>
   登陆组件
 </div>
</template>
<!--行为区-->
<script>
export default {

  data () {
    return {
      // 这是登录表单的数据绑定对象
      loginForm: {
        username: 'admin',
        password: '123456'
      },
      // 这是表单的验证规则对象
      loginFormRules: {
        username: [
          { required: true, message: '请输入登录名', trigger: 'blur' },
          { min: 3, max: 10, message: '长度在 3 到 10 个字符', trigger: 'blur' }
        ],
        password: [
          { required: true, message: '请输入登录密码', trigger: 'blur' },
          { min: 6, max: 15, message: '长度在 6 到 15 个字符', trigger: 'blur' }
        ]
      }
    }
  },
  methods: {
    resetLoginForm () {
      // console.log(this)
      this.$refs.loginFormRef.resetFields()
    },
    login () {
      this.$refs.loginFormRef.validate(async valid => {
        if (!valid) return
        // console.log(valid) 登陆验证
        const { data: res } = await this.$http.post('login', this.loginForm)
        if (res.meta.status !== 200) return this.$message.error('登录失败')
        this.$message.success('登录成功')
        console.log(res)
        window.sessionStorage.setItem('token', res.data.token)
        this.$router.push('/home')
      //  1.登录成功之后的 token, 保存到客户端的sessiStorage中
      //  1.1 项目中给了登录的其他API接口 必须在登录之后才能访问
      //  1.2 token 只应在当前网站打开期间生效 所以将token保存在sessioStorage中(仅仅在会话层面，localStorage 是持久性的）
      //  通过编程式 导航跳转到后台主页中，路由地址是/home
      })
    }
  }
}
</script>
<style lang="less" scoped>
//支持less 防止组件之间冲突
.login_container{
  background-color: #2b4b6b;
  height: 100%;
}
.loin_box{
  width: 450px;
  height: 300px;
  background-color: #fff;
  border-radius: 3px;
  position: absolute;
  left: 50%;
  top: 52%;
  transform: translate(-50%, -50%);
  .avatar_box{
  height: 130px;
  width: 130px;
  border: 1px solid #eee;
  border-radius: 50%;
  padding: 10px; // 图片和表框有了间隙
  box-shadow: 0 0 10px #ddd;
  position: absolute;
  left: 50%;
  transform: translate(-50%,-50%);
  background-color: #fff;
  img{
    width: 100%;
    height: 100%;
    border-radius: 50%;
    background-color: #eee;

  }
  }
}
.login_form{
  position: absolute;
  bottom: 0;
  width: 100%;
  padding: 0 20px;
  box-sizing: border-box;
}
.btns{
  display: flex;
  justify-content: flex-end;
}
</style>

<!--防止组件之间的相互冲突-->
