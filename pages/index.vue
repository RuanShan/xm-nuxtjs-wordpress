<template>
  <div class="container">
    <!-- banner start -->
    <div class="banner-wrap">
      <div class="big-banner">
        <a class="list block" :href="info.banner.big_banner.link">
          <img :src="info.banner.big_banner.path" alt="">
          <span
            class="title"
            :title="info.banner.big_banner.text"
            v-show="info.banner.big_banner.text">
            {{ info.banner.big_banner.text }}
          </span>
        </a>
      </div>
      <ul class="small-banner">
        <li class="list" v-for="item in info.banner.small_banner" :key="item.key">
          <a class="block" :href="item.link">
            <img :src="item.path" alt="">
            <span v-show="item.text" class="title" :title="item.text">{{ item.text }}</span>
          </a>
        </li>
      </ul>
    </div>
    <!-- banner end -->
    <div v-if="info.notice" class="sidebar-list notice tablet-show">
      <div class="header">
        <p>
          <x-icon type="icon-notice2"></x-icon> 公告
        </p>
      </div>
      <div class="content" v-html="info.notice"></div>
    </div>
    <!-- article list start -->
    <div class="article-list-wrap">
      <ul class="header">
        <li class="list">最新文章</li>
      </ul>
      <article class="article-list" v-for="item in articleList" :key="item.key">
        <nuxt-link :to="{ name: 'details-id', params: { id: item.id } }" class="thumbnail-wrap">
          <img :src="item.articleInfor.thumbnail" class="thumbnail" alt="">
        </nuxt-link>
        <div class="list-content">
          <h2 class="title">
            <nuxt-link :to="{ name: 'details-id', params: { id: item.id } }" v-html="item.title.rendered"></nuxt-link>
          </h2>
          <p class="summary">{{ item.articleInfor.summary }}</p>
          <div class="opeartion">
            <div class="information">
              <span><x-icon type="icon-date"></x-icon>{{ item.date }}</span>
              <span><x-icon type="icon-hot1"></x-icon>{{ item.articleInfor.viewCount }}</span>
              <span><x-icon type="icon-message"></x-icon>{{ item.articleInfor.commentCount }}</span>
              <span><x-icon type="icon-good"></x-icon>{{ item.articleInfor.xmLike.very_good }}</span>
            </div>
            <nuxt-link class="details-btn" :to="{ name: 'details-id', params: { id: item.id } }">阅读详情</nuxt-link>
          </div>
        </div>
      </article>
      <!-- more btn start -->
      <el-pagination
        small
        :page-size="8"
        layout="prev, pager, next, jumper"
        :current-page="currentPage"
        @current-change="_changePagination"
        :total="totalPage">
      </el-pagination>
      <!-- more btn end -->
    </div>
    <!-- article list end -->
  </div>
</template>

<script>
import { mapState } from 'vuex'
export default {
  name: 'Index',
  fetch ({ store }) {
    store.commit('article/SET_CURRENT_PAGE', 1)
    return store.dispatch('article/getArticleList', {
      page: 1,
      per_page: 8,
      _embed: true
    })
  },
  computed: {
    ...mapState(['info']),
    ...mapState('article', ['articleList', 'totalPage', 'currentPage'])
  },
  head () {
    return {
      title: `${this.info.blogName} | ${this.info.blogDescription}`,
      meta: [
        { name: 'keywords', content: this.info.keywords },
        { name: 'description', content: this.info.description }
      ]
    }
  },
  methods: {
    _changePagination (id) {
      this.$router.push({
        name: 'article-id-title',
        params: { id }
      })
    }
  }
}
</script>

<style lang="scss" scoped>
// banner
.banner-wrap {
  display: flex;
  justify-content: space-between;
  height: 320px;

  img {
    vertical-align: top;
    width: 100%;
    height: 100%;
  }

  .list {
    overflow: hidden;
    position: relative;
    border-radius: $border-radius;

    .title {
      position: absolute;
      bottom: 0;
      left: 0;
      width: 100%;
      height: 30px;
      background: rgba(0,0,0,.3);
      text-indent: $font-size-base;
      line-height: 30px;
      color: $color-white;
      @extend %ellipsis;
    }
  }

  .big-banner {
    width: 710px;

    img {
      height: 320px;
    }
  }

  .small-banner {
    width: 180px;

    .list {
      height: 100px;
      margin-bottom: 10px;

      &:last-of-type {
        margin-bottom: 0;
      }
    }
  }
}

@media screen and (max-width: 1024px) {
  .banner-wrap {
    flex-wrap: wrap;
    height: auto;

    .big-banner {
      width: 100%;

      img {
        height: auto;
      }
    }

    .small-banner {
      display: flex;
      justify-content: space-between;
      width: 100%;
      margin-top: $container-margin;

      .list {
        width: 32%;
        height: auto;
        margin-bottom: 0;
      }

      img {
        height: auto;
      }
    }
  }
}

@media screen and (max-width:767px) {
  .banner-wrap .list .title {
    height: 20px;
    font-size: 10px;
    line-height: 20px;
    text-indent: 8px;
  }
  // 文章列表
  .article-list-wrap {
    .article-list {
      flex-wrap: wrap;
      height: auto;

      .title {
        margin-top: 15px;
        font-size: $font-size-large;
      }

      .summary {
        height: auto;
      }

      .list-content {
        height: auto;
      }

      .opeartion {
        position: static;
        display: block;
        margin-top: 10px;
      }

      .details-btn {
        display: block;
        margin-top: 10px;
        padding: 10px 0;
        text-align: center;
      }
    }

    .thumbnail-wrap {
      width: 100%;
      margin-right: 0;
      text-align: center;

      .thumbnail {
        width: auto;
        height: auto;
        max-height: 150px;
      }
    }
  }

  // 翻页
  /deep/ .el-pagination {
    .el-pagination__jump {
      display: none;
    }
  }
}
</style>
