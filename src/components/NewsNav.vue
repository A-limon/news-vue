<template>
  <nav class="nav l-flex">
    <div class="item" @click="jump('foryou')">
      <p><i class="iconfont">&#xe643;</i></p>
      <p>For You</p>
    </div>
    <div class="item" @click="jump('channels')">
      <p><i class="iconfont">&#xe608;</i></p>
      <p>Channels</p>
    </div>
    <div class="item" @click="jump('explore')">
      <p><i class="iconfont">&#xe614;</i></p>
      <p>Explore</p>
    </div>
    <div class="item" @click="jump('setting')">
      <p><i class="iconfont">&#xe600;</i></p>
      <p>Setting</p>
    </div>
  </div>
</template>

<script>
import router from '../router'

export default {
  name: 'NewsNav',
  methods: {
    jump (nav) {
      switch (nav) {
        case 'channels':
          router.push({ name: 'Channels'})
          break
        case 'foryou':
          router.push({ name: 'ForYou'})
          break
        case 'explore':
          router.push({ name: 'Explore'})
          break
        case 'setting':
          router.push({ name: 'Setting'})
          break
      }
    }
  }
}
</script>

<style scoped>
  .nav {
    width: 10rem;
    height: 1.2rem;
    position: fixed;
    bottom: 0;
    left: 0;
    background: rgba(255, 255, 255, 0.9);
    border-top: 1px solid #ccc;
    align-items: center;
    overflow: hidden;
    user-select: none;
    color: #757373;
  }
  .nav .item {
    flex: 1;
    text-align: center;
  }
</style>
