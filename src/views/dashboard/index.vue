<template>
  <div class="dashboard-container">
    <div class="dashboard-text">当前管理员: {{ name }}</div>
    <el-card>
      <h2>基于 <el-link type="primary" href="https://github.com/PanJiaChen/vue-admin-template">vue-admin-template</el-link> 搭建的个人博客管理后台</h2>
      <h2>前台博客地址： <el-link type="primary" href="http://waiyed.gitee.io/vue-blog/#/">点击跳转</el-link></h2>
      <h2>后台页面GitHub：<el-link type="primary" href="http://waiyed.gitee.io/vue-blog/#/">点击跳转</el-link></h2>
      <p>后台页面主要使用 Vue + element-ui + echarts</p>
      <p>该后台数据均是真实数据，后台服务器部署在阿里云，后台服务器主要使用 Node.js + mongodb，主要库使用 express + mongoose</p>
    </el-card>
    <el-card>
      <div>
        <pie-chart :chart-data="PieChartData" />
      </div>
    </el-card>
  </div>
</template>

<script>
import { mapGetters } from 'vuex'
import { getBlogCount } from '@/api/table'
import PieChart from './components/PieChart'

export default {
  name: 'Dashboard',
  data() {
    return {
      blogCount: 10,
      PieChartData: {
        blogCount: 0,
        userBlog: 0
      }
    }
  },
  components: {
    PieChart
  },
  computed: {
    ...mapGetters([
      'name'
    ])
  },
  created() {
    this.fetchPieChartData()
  },
  methods: {
    async fetchPieChartData() {
      let { data } = await getBlogCount(this.name)
      this.PieChartData.blogCount = data.blog_count
      this.PieChartData.userBlog = data.user_blog
    }
  }
}
</script>

<style lang="scss" scoped>
.dashboard {
  &-container {
    margin: 30px;
  }
  &-text {
    font-size: 30px;
    line-height: 46px;
  }
}
</style>

<style lang="scss">
h2 > .el-link.el-link--primary {
  font-size: 24px;
}
</style>
