<template>
  <div class="mcontaner">
    <Header></Header>

    <div class="block" v-if="length!==0">
      <el-timeline>
        <el-timeline-item
          :timestamp="blog.created"
          placement="top"
          v-for="(blog, index) in blogs"
          :key="index"
        >
          <el-card>
            <h4>
              <router-link :to="{name: 'BlogDetail', params: {blogId: blog.id}}">{{blog.title}}</router-link>
            </h4>
            <p>{{blog.description}}</p>
          </el-card>
        </el-timeline-item>
      </el-timeline>

      <el-pagination
        class="mpage"
        background
        layout="prev, pager, next"
        :current-page="currentPage"
        :page-size="pageSize"
        :total="total"
        @current-change="page"
      ></el-pagination>
    </div>
    <el-empty v-else description="博主还没有发布内容哦！"></el-empty>
  </div>
</template>

<script>
import Header from "../components/Header";

export default {
  name: "Blogs.vue",
  components: { Header },
  data() {
    return {
      blogs: {},
      currentPage: 1,
      total: 0,
      pageSize: 5,
      length: 0
    };
  },
  methods: {
    page(currentPage) {
      const _this = this;
      _this.$axios
        .get("/blog/query?current=" + currentPage + "&size=" + this.pageSize)
        .then(res => {
          _this.blogs = res.data.data.records;
          _this.length = res.data.data.records.length;
          _this.currentPage = res.data.data.current;
          _this.total = res.data.data.total;
          _this.pageSize = res.data.data.size;
        });
    }
  },
  created() {
    this.page(1);
  }
};
</script>

<style scoped>
.mpage {
  margin: 0 auto;
  text-align: center;
}
</style>