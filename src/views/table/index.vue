<template>
  <div class="app-container">
    <el-table
      v-loading="listLoading"
      :data="list"
      element-loading-text="Loading"
      border
      fit
      highlight-current-row
    >
      <el-table-column
        prop="create_time"
        label="日期"
        width="180">
      </el-table-column>
      <el-table-column
        prop="author_name"
        label="作者"
        width="180">
      </el-table-column>
      <el-table-column
        prop="blog_title"
        label="博客标题"
        width="180">
      </el-table-column>
      <el-table-column
        label="分类"
        width="180">
        <template slot-scope="scope">
          <div 
            v-for="item in scope.row.blog_type" 
            :key="item"
            class="float-left">
            <el-tag
              :type="item | statusFilter"
              disable-transitions>{{item}}</el-tag>
          </div>
        </template>
      </el-table-column>
      <el-table-column
        label="博客正文">
        <template slot-scope="scope">
            <div 
              v-html="scope.row.source_article" 
              class="table-row-article">
            </div>
        </template>
      </el-table-column>
    </el-table>
  </div>
</template>

<script>
import { getList } from '@/api/table'

export default {
  filters: {
    statusFilter(status) {
      const statusMap = {
        'web前端': 'success',
        '日常': 'gray',
        'web后端': 'danger'
      }
      return statusMap[status]
    }
  },
  data() {
    return {
      list: null,
      listLoading: true
    }
  },
  created() {
    this.fetchData()
  },
  methods: {
    fetchData() {
      this.listLoading = true
      getList().then(response => {
        this.list = response.data.items.reverse()
        this.listLoading = false
      })
    }
  }
}
</script>

<style lang="scss" scoped>
.table-row-article {
  max-height: 300px;
  overflow: hidden;
}
.float-left {
  float: left;
  margin: 0 10px 5px 0;
}
</style>