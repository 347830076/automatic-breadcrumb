<!-- 面包屑组件 -->
<style scoped lang="scss">
.breadcrumb-page {
  display: flex;
  align-items: center;
  color: #8C8C8C;

  span {
    color: #8C8C8C;
    cursor: pointer;

    &:hover {
      color:#11A560;
    }
  }

  .theme-color {
    font-weight: 600;
    color: #11A560;
  }
}
</style>

<template>
  <section class="breadcrumb-page">
    当前位置：
    <div v-for="(item, index) in breadList" :key="index">
      <span
        v-if="breadList.indexOf(item) === breadList.length - 1"
        class="theme-color"
      >
        {{ item.meta.title }}
      </span>
      <span v-else  @click="goto(item, index)">
        {{ item.meta.title }} >
      </span>
    </div>
  </section>
</template>

<script>
export default {
  name: 'automatic-breadcrumb',
  data () {
    return {
      breadList: [] // 路由集合
    }
  },

  watch: {
    $route () {
      this.getBreadcrumb()
    }
  },

  created () {
    this.getBreadcrumb()
  },

  methods: {
    goto (item, index) {
      let temp = ''
      try {
        temp = JSON.parse(this.breadList[index + 1]?.query?.pQuery)
      } catch (error) {
      }
      this.$router.push({
        path: item.path,
        query: item.query || temp
      })
    },
    getBreadcrumb () {
      const { matched, query, path } = this.$route
      this.breadList = matched
        .map(r => {
          r.path = r.path ? r.path : '/'
          if (path === r.path) {
            r.query = query
          }
          return r
        })
        .filter(r => r.meta.title)
    }
  }
}
</script>
