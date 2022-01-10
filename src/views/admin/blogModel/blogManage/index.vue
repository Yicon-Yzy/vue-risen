<template>
  <div class="app-container">
		<el-row>
			<!-- <el-button>默认按钮</el-button> -->
			<router-link to="/article/articleList" style="margin:0 10px 0 10px">
				<el-button type="primary">
					文章列表
				</el-button>
			</router-link>
			<router-link to="/article/articleType" style="margin:0 10px 0 10px">
				<el-button type="primary">
					文章类型
				</el-button>
			</router-link>
			<router-link to="/article/friendlyLink" style="margin:0 10px 0 10px">
				<el-button type="primary">
					友情链接
				</el-button>
			</router-link>
		</el-row>
  </div>
</template>

<script>

export default {
  data () {
    return {
    }
  },
  mounted () {
  },
  methods: {
  }
}
</script>

<style scoped lang="scss">
</style>
