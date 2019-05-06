<template>
  <main class="page">
    <slot name="top"/>
    <div class="content default">
      <h1>{{title}}</h1>
      <div class="categories">
        <Category @categoryItem="categoryData"/>
      </div>
      <div class="tags">
        <Tags @tagsItem="categoryData"/>
      </div>
      <div>
        <small>共{{list.length}}篇</small>
      </div>
      <div class="list">
        <section v-for="(item, index) in list" :key="index" class="mb15 row">
          <article :style="{padding:'0 20px','border-right':'5px solid'+ randomColor(index)}">
            <header>
              <h3 class="m0 title">
                <router-link :to="item.path" class="top-bottom">{{ item.title }}</router-link>
              </h3>
            </header>
            <div class="info">
              <span>
                <i class="iconfont icon-shijian"></i>
                <span class="category">{{item.frontmatter.date || '2019/05/55'}}</span>
              </span>
              <span
                :id="item.path"
                class="leancloud-visitors vistorNum category"
                data-flag-title="Your Article Title"
              >
                <!-- <em class="post-meta-item-text">
                </em>-->
                <i class="iconfont icon-eye"></i>
                <i class="leancloud-visitors-count">0</i>
              </span>

              <span>
                <i class="iconfont icon-leimupinleifenleileibie"></i>
                <span class="category">{{item.frontmatter.categories[0]}}</span>
              </span>

              <span>
                <i class="iconfont icon-tag"></i>
                <span v-for="(tag, i) in item.frontmatter.tags " :key="i" class="tag">
                  <span v-if="i!=0">、</span>
                  {{tag}}
                </span>
              </span>
            </div>
          </article>
        </section>
      </div>
    </div>
    <Content/>
    <div v-show="false">
      <Valine/>
    </div>
    <slot name="bottom"/>
  </main>
</template>

<script>
import { resolvePage, outboundRE, endingSlashRE } from '../util'
import Category from '@theme/components/Category.vue'
import Tags from '@theme/components/Tags.vue'
import Valine from '@theme/components/Valine.vue'

export default {
  props: ['sidebarItems'],
  data() {
    return {
      title: '文章列表',
      list: []
    }
  },
  created() {
    this.initList()
  },
  components: {
    Tags,
    Category,
    Valine
  },
  watch: {
    $route(to, from) {
      if (to.path !== from.path) {
        this.initList()
      }
    }
  },
  computed: {
    lastUpdated() {
      return this.$page.lastUpdated
    },

    lastUpdatedText() {
      if (typeof this.$themeLocaleConfig.lastUpdated === 'string') {
        return this.$themeLocaleConfig.lastUpdated
      }
      if (typeof this.$site.themeConfig.lastUpdated === 'string') {
        return this.$site.themeConfig.lastUpdated
      }
      return 'Last Updated'
    },
    categories() {
      return this.$categories._metaMap
    },
    tags() {
      return this.$tags._metaMap
    },
    isTag() {
      return this.$frontmatter.isTag
    },
    isCategory() {
      return this.$frontmatter.isCategory
    },
    colors() {
      return this.$themeConfig.colors
    }
  },

  methods: {
    initList() {
      const order = (a, b) => {
        const x = new Date(a.frontmatter.date).getTime()
        const y = new Date(b.frontmatter.date).getTime()
        if (x < y) {
          return 1
        } else if (x > y) {
          return -1
        } else {
          return 0
        }
      }
      if (this.isCategory) {
        let arr = []
        for (const key in this.categories) {
          if (this.categories.hasOwnProperty(key)) {
            const { posts } = this.categories[key]
            arr = arr.concat(posts)
          }
        }
        // console.log(arr)
        this.list = arr.sort(order)
      } else {
        this.list = []
      }
    },
    categoryData(type, data) {
      if (type === 'all') {
        this.initList()
        this.title = '文章列表'
      } else {
        this.title = '文章列表--' + type
        this.list = data.posts
      }
    },
    randomNum() {
      return Math.floor(Math.random() * this.colors.length)
    },
    randomColor(val) {
      return this.colors[val % this.colors.length]
    }
  }
}
</script>

<style lang="stylus">
$lightColor = #eaecef;

.page {
  padding-bottom: 2rem;
  display: block;

  a {
    color: #303133;
  }
}

.mb10 {
  margin-bottom: 10px;
}

.mb15 {
  margin-bottom: 15px;
}

.tag {
  padding: 3px 5px;
  margin-bottom: 5px;
  font-size: 12px;
}

.list {
  padding: 20px 0;
}

.tagList {
  border-left: 1px solid $lightColor;
  border: 1px solid $lightColor;
  border-top: 0;
}

.category {
  padding: 3px 5px;
  margin-bottom: 5px;
  margin-right: 10px;
  font-size: 12px;
  // border: 1px solid #000;
}

.vistorNum {
  font-size: 12px;
}

.row {
  box-shadow: 2px 2px 5px $lightColor;
  border: 1px solid $lightColor;
  transition all .5s
  &:hover{
    transform scale(1.05)
  }
  .title {
    border-bottom: 1px solid $lightColor;
    // padding: 10px 20px;
    padding: 10px 0;

    a:hover {
      text-decoration: none;
    }
  }

  .info {
    padding: 10px 0;
  }
}

.m0 {
  margin: 0;
}

.mb5 {
  margin-bottom: 5px;
}

.dashedBox {
  border: 2px dashed $lightColor;
  border-radius: 4px;
}
</style>
