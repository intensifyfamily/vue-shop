<template>
  <el-container class="home-container">
    <!--头部区域-->
    <el-header>
      <div>
        <img src="../assets/images/head.jpg" width="60" height="60" alt="">
        <span>电商后台管理系统</span>
      </div>
      <el-button type="info" @click="logout">退出</el-button>
    </el-header>
      <!--页面主题区域-->
    <el-container>
      <!--侧边栏-->
      <el-aside :width="isCollapse ? '64px' : '200px'">
        <div class="toggle-button" @click="toggleCollapse">|||</div>
        <!--侧边栏菜单区-->
        <el-menu
          background-color="#333744"
          text-color="#fff"
          active-text-color="#409EFF"
          unique-opened
          :collapse="isCollapse"
          :collapse-transition="false"
          router
          :default-active="activePath">
          <!--一级菜单-->
          <el-submenu :index="item.id + ''" v-for="item in menulist" :key="item.id">
            <!--一级菜单的模板-->
            <template slot="title">
              <!--图标-->
              <i :class="iconsObj[item.id]"></i>
              <!--文本-->
              <span>{{ item.authName }}</span>
            </template>
            <!--二级菜单-->
            <el-menu-item :index="'/'+subItem.path" v-for="subItem in item.children" :key="subItem.id" @click="saveNavState('/'+subItem.path)">
              <template slot="title">
                <!--图标-->
                <i class="el-icon-menu"></i>
                <!--文本-->
                <span>{{ subItem.authName }}</span>
            </template>
            </el-menu-item>
          </el-submenu>
        </el-menu>
      </el-aside>
      <!--右侧内容主体-->
      <el-main>
        <!--路由占位符-->
        <router-view></router-view>
      </el-main>
    </el-container>
  </el-container>
</template>

<script>
export default {
  data () {
    return {
      // 左侧菜单数据
      menulist: [
        { id: 125, authName: '成员管理', path: 'rightsds', children: [{ id: 3, authName: '权sddd', path: 'users', children: [], order: 1 }], order: 2 },
        {
          id: 103,
          authName: '权限管理',
          path: 'rights',
          children: [
            { id: 1, authName: '权限列表', path: 'rights', children: [], order: 1 },
            { id: 2, authName: '角色列表', path: 'roles', children: [], order: 1 }
          ],
          order: 1
        },
        {
          id: 104,
          authName: '商品管理',
          path: 'categories',
          children: [
            { id: 3, authName: '商品列表', path: '', children: [], order: 1 },
            { id: 4, authName: '分类参数', path: 'params', children: [], order: 2 },
            { id: 5, authName: '商品分类', path: 'categories', children: [], order: 3 }
          ],
          order: 1
        }
      ],
      iconsObj: {
        125: 'iconfont iconusers',
        103: 'iconfont icontijikongjian',
        101: 'iconfont iconshangpin',
        102: 'iconfont icondanju',
        145: 'iconfont iconbaobiao'
      },
      isCollapse: false,
      // 被激活的连接地址
      activePath: ''
    }
  },
  created () {
    this.getMenuList()
    this.activePath = window.sessionStorage.getItem('activePath')
  },
  methods: {
    logout () {
      window.sessionStorage.clear()
      this.$router.push('/login')
    },
    // 获取所有的菜单
    async getMenuList () {
      const { data: res } = await this.$http.get('menu')
      // if (res.meta.data !== 200) return this.$message.error(res.meta.msg)
      console.log(res.data)
    },
    // 点击按钮实现左侧菜单折叠展开
    toggleCollapse () {
      this.isCollapse = !this.isCollapse
    },
    // 保存连接的激活状态
    saveNavState (activePath) {
      window.sessionStorage.setItem('activePath', activePath)
      this.activePath = activePath
    }
  }
}

</script>

<style lang="less" scoped>
.home-container {
  height: 100%;
}

.el-header {
  background-color: #373d41;
  display: flex;
  justify-content: space-between;
  padding-left: 0;
  align-items: center;
  color: #fff;
  font-size: 20px;
  > div {
    display: flex;
    align-items: center;
    span {
      margin-left: 15px;
    }
  }
}

.el-aside {
  background-color: #333744;
  .el-menu {
    border-right: 0;
  }
}

.el-main {
  background-color: #eaedf1;
}

.iconfont {
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
