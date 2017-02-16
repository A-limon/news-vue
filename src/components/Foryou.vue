<template>
  <div class="foryou">
    <template v-if="showSubscribed">
      <template v-if="subscribedLoaded">
        <h1>For You</h1>
        <template v-for="item in subscribedNewsToShow">
          <Subscribed :item="item"></Subscribed>
        </template>
      </template>
    </template>
    <template v-else>
      <h1>Subscribe News Now</h1>
      <h2 class="section l-flex" @click="jumpTo('channels')">
        <span class="title">Channels</span>
        <span class="more"><i class="iconfont">&#xe60e;</i></span>
      </h2>
      <p class="desc">We Have More Than 60 Channels</p>
      <template v-if="channelsLoaded">
        <div class="channels-list margin l-flex">
          <template v-for="item in channelsList">
            <ChannelItem :item="item"></ChannelItem>
          </template>
        </div>
      </template>

      <h2 class="section l-flex" @click="jumpTo('explore')">
        <span class="title">Explore</span>
        <span class="more"><i class="iconfont">&#xe60e;</i></span>
      </h2>
      <p class="desc">News All Round World</p>
      <template v-if="newsLoaded">
        <div class="channels-list l-flex">
          <template v-for="item in newsListToShow">
            <HotNews :item="item"></HotNews>
          </template>
        </div>
      </template>
    </template>
  </div>
</template>

<script>
import axios from 'axios'
import router from '../router'
import Subscribed from './Subscribed'
import ChannelItem from './ChannelItem'
import HotNews from './HotNews'

export default {
  name: 'ForYou',
  data () {
    return {
      subscribedLoaded: false,
      channelsLoaded: false,
      newsLoaded: false,
      isFetching: false,
      newsList: [],
      channelsList: [],
      subscribedNewsList: [],
      subscribedList: window.SubscribedList
    }
  },
  computed: {
    showSubscribed () {
      if (window.NotSupportedLs || this.$data.subscribedList.length === 0) {
        return false
      } else {
        return true
      }
    },
    subscribedNewsToShow () {
      if (!this.showSubscribed) {
        return []
      } else {
        const res = []
        this.$data.subscribedNewsList.forEach(function (item) {
          const news = {}
          news.name = item.source.replace(/(-)/,' ').toUpperCase()
          news.list = item.content
          res.push(news)
        })
        return res
      }
    },
    newsListToShow () {
      if (this.showSubscribed) {
        return []
      } else {
        const res = []
        this.$data.newsList.forEach(function (item) {
          const news = {}
          news.news = item
          res.push(news)
        })
        return res
      }
    }
  },
  components: {
    Subscribed,
    ChannelItem,
    HotNews
  },
  beforeRouteEnter  (to, from, next) {
    document.title = 'Headline News For You'
    next()
  },
  methods: {
    jumpTo (name) {
      switch (name) {
        case 'channels':
          router.push({ name: 'Channels'})
        break
        case 'explore':
          router.push({ name: 'Explore'})
        break
      }
    },
    fetchSubscribed () {
      axios.post(SETTING.NewsAPI + '/multi', {
        ids: this.$data.subscribedList
      })
      .then(function (response) {
        if (response.data && response.data.code === 1 && response.data.data.length > 0) {
          this.$data.subscribedNewsList = response.data.data
          this.$data.subscribedLoaded = true
          this.$data.isFetching = false
        }
      }.bind(this))
      .catch(function (error) {
        console.error(error)
      })
    },
    fetchChannels () {
      this.isFetching = true
      axios.get(SETTING.ChannelsAPI + '/random')
      .then(function (response) {
        if (response !== undefined && response.data.code === 1) {
          this.$data.channelsList = response.data.data
          this.$data.channelsLoaded = true
          this.$data.isFetching = false
        }
      }.bind(this))
      .catch(function (error) {
        console.error(error)
      })
    },
    fetchNews () {
      this.isFetching = true
      axios.get(SETTING.NewsAPI + '/random')
      .then(function (response) {
        if (response !== undefined && response.data.code === 1) {
          this.$data.newsList = response.data.data
          this.$data.newsLoaded = true
          this.$data.isFetching = false
        }
      }.bind(this))
      .catch(function (error) {
        console.error(error)
      })
    }
  },
  created () {
    if (this.showSubscribed) {
      this.fetchSubscribed()
    } else {
      this.fetchChannels()
      this.fetchNews()
    }
    eventBus.$on('reload-cur-news', function (name) {
      if (name === 'foryou') {
        this.fetchNews()
      }
    }.bind(this))
  },
}
</script>

<style scoped>
  .foryou h1 {
    font-size: 30px;
    width: 9.2rem;
    padding: 0.266667rem 0.4rem 0 0.4rem;
    color: #3ca3c7;
    font-weight: bold;
  }
  [data-dpr="2"] .foryou h1 {
    font-size: 60px;
  }
  [data-dpr="3"] .foryou h1 {
    font-size: 90px;
  }
  .foryou .section {
    width: 9.2rem;
    padding: 0.133333rem 0.4rem 0 0.4rem;
    align-items: flex-end;
    justify-content: space-between;
  }
  .foryou .desc {
    width: 9.2rem;
    padding: 0 0.4rem 0 0.4rem;
    color: #ccc;
  }
  .foryou .section .iconfont {
    color: #3ca3c7;
    font-size: 26px;
    line-height: 1;
  }
  .foryou .section .title {
    font-size: 26px;
    color: #3ca3c7;
    font-weight: bold;
    margin-top: 0.266667rem;
  }
  [data-dpr="2"] .foryou .section .title,
  [data-dpr="2"] .foryou .section .iconfont {
    font-size: 52px;
  }
  [data-dpr="3"] .foryou .section .title,
  [data-dpr="3"] .foryou .section .iconfont {
    font-size: 78px;
  }
  .channels-list {
    width: 10rem;
    overflow: hidden;
    align-content: center;
    justify-content: space-around;
    flex-wrap: wrap;
  }
  .channels-list.margin {
    margin-bottom: 0.5rem;
  }
</style>
