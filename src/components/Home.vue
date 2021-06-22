<!--组件要想使用的话必须注册一下-->
<!--动态绑定 在组件-->
<template>
  <el-container class="home-container">
    <!--      el-head 头部区域-->
    <el-header>
      <div>
        <img src="../assets/heima.png" alt="">
        <span>电商后台管理系统</span>
      </div>
      <el-button type="info" @click="logout">退出</el-button>
    </el-header>
    <!--      页面主体区-->
    <el-container>
      <!--        侧边区域- 用isCollapes设置是否折叠-->
      <el-aside :width="isCollapse? '64px':'200px'">
        <div class="toggle-button" @click="toggleCollapse">|||</div>
<!--        active-text-color 激活的文本值 default-active 表示的是进行高亮的menu项目-->
        <el-menu background-color="#333744" text-color="#fff"
                 active-text-color="#409EFF" :unique-opened ="true"
                 :collapse="isCollapse" :collapse-transition="false"
                 :router="true" :default-active="activePath">
<!--          一级菜单 -->
<!--         渲染一级菜单 这里要注意的是由于index 只接受字符串不接受 数值 因此加一个空格可以转化为字符串-->
          <el-submenu :index="item.id + ''" v-for= " item in menulist" :key=" item.id">
<!--            一级菜单的模板区域-->
            <template slot="title">
<!--              图标-->
              <i :class="inconsObj[item.id]"></i>
<!--              文本-->
              <span>{{item.authName}}</span>
            </template>
            <!--二级菜单加上斜线可以实现侧边栏的路由改造-->
              <el-menu-item :index="'/' + subItem.path +''" v-for=" subItem in item.children"
                            :key="subItem.id" @click="saveNavStatus('/'+ subItem.path)">
                <!--            二级菜单的模板区域-->
                <template slot="title">
                  <!--              图标-->
                  <i class="el-icon-menu"></i>
                  <!--              文本-->
                  <span>{{subItem.authName}}</span>
                </template>
              </el-menu-item>
          </el-submenu>
        </el-menu>
      </el-aside>
      <!--        右侧内容-->
      <el-main>
<!--        路由占位符-->
        <router-view></router-view>
      </el-main>
    </el-container>
  </el-container>
</template>

<script>
export default {
  name: 'Home',
  data () {
  //  左侧菜单数据 保存在menulist 中
    return {
      menulist: [],
      inconsObj: {
        125: 'iconfont icon-user',
        103: 'iconfont icon-tijikongjian',
        101: 'iconfont icon-shangpin',
        102: 'iconfont icon-danju',
        145: 'iconfont icon-baobiao'
      },
      // 是否折叠 fales
      isCollapse: false,
      //  被激活的链接地址
      activePath: ' '
    }
  },
  created () {
    this.getMenuList()
    this.activePath = window.sessionStorage.getItem('activePath')
  },
  methods: {
    logout () {
      // 退出功能 清空token，重定向到login
      window.sessionStorage.clear()
      this.$router.push('/login')
    },
    // 获取所有的菜单
    async getMenuList() {
      const { data: res } = await this.$http.get('menus')
      if (res.meta.status !== 200) return this.$message.error(res.meta.msg)
      this.menulist = res.data
      console.log(res)
    },
    //  点击按钮切换菜单的折叠与展开
    toggleCollapse() {
      this.isCollapse = !this.isCollapse
    },
    // 保存链接的激活状态
    saveNavStatus(activePath) {
      window.sessionStorage.setItem('activePath', activePath)
      this.activePath = activePath
    }
  }
}

</script>
<style lang="less" scoped>

.home-container{ height: 100% }
.el-header { background-color: #373D41;
  display: flex;
  justify-content: space-between;
  padding-left: 0;
  align-items: center;
  color: #ffffff;
  font-size: 20px;
  > div{
    display: flex;
    align-items: center;
    span{
      margin-left: 10px;
    }
  }
}
.el-aside { background-color: #333744;
  .el-menu {
    border-right: 0px;
}
}
.el-main {background-color: #EAEDF1;}
.el-button { right: revert}
.iconfont{
margin-right: 10px;
}
.toggle-button {
  background-color: #4A5064;
  font-size: 10px;
  line-height: 24px;
  color: #fff;
  text-align: center;
  letter-spacing: 0.2em;
  cursor: pointer;
}
</style>
