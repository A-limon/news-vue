<template>
  <div class="channels l-flex">
    <h1>Channels</h1>
    <template v-for="item in list">
      <ChannelItem :item="item"></ChannelItem>
    </template>
  </div>
</template>

<script>
import axios from 'axios'
import ChannelItem from './ChannelItem'

export default {
  name: 'Channels',
  data () {
    return {
      loaded: false,
      isFetching: false,
      list: {}
    }
  },
  components: {
    ChannelItem
  },
  beforeRouteEnter  (to, from, next) {
    document.title = 'Channels'
    next()
  },
  methods: {
    fetchChannels () {
      this.isFetching = true
      axios.get(SETTING.ChannelsAPI)
      .then(function (response) {
        if (response !== undefined && response.data.code === 1) {
          this.$data.list = response.data.data
          this.$data.loaded = true
          this.$data.isFetching = false
        }
      }.bind(this))
      .catch(function (error) {
        console.error(error)
      })
    }
  },
  created () {
    this.fetchChannels()
  }
}
</script>

<style scoped>
  .channels {
    width: 10rem;
    overflow: hidden;
    align-content: center;
    justify-content: space-around;
    flex-wrap: wrap;
  }
  .channels h1 {
    font-size: 30px;
    width: 9.2rem;
    padding: 0.266667rem 0.4rem 0 0.4rem;
    color: #f14747;
    font-weight: bold;
  }
  [data-dpr="2"] .channels h1 {
    font-size: 60px;
  }
  [data-dpr="3"] .channels h1 {
    font-size: 90px;
  }
</style>
