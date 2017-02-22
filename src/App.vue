<template>
  <div id="app">
    <router-view></router-view>
    <NewsNav></NewsNav>
    <Loading></Loading>
  </div>
</template>

<script>
import NewsNav from './components/NewsNav'
import Loading from './components/Loading'
export default {
  name: 'app',
  components: {
    NewsNav,
    Loading
  }
}
</script>

<style>
  @font-face {font-family: "iconfont";
    src: url('../static/font/iconfont.eot?t=1487751947737'); /* IE9*/
    src: url('../static/font/iconfont.eot?t=1487751947737#iefix') format('embedded-opentype'), /* IE6-IE8 */
    url('../static/font/iconfont.woff?t=1487751947737') format('woff'), /* chrome, firefox */
    url('../static/font/iconfont.ttf?t=1487751947737') format('truetype'), /* chrome, firefox, opera, Safari, Android, iOS 4.2+*/
    url('../static/font/iconfont.svg?t=1487751947737#iconfont') format('svg'); /* iOS 4.1- */
  }
  .iconfont {
    font-family: "iconfont" !important;
    font-size: 20px;
    font-style: normal;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
  }
  .iconfont.big {
    font-size: 26px;
  }
  [data-dpr="2"] .iconfont {
    font-size: 40px;
  }
  [data-dpr="2"] .iconfont.big {
    font-size: 52px
  }
  [data-dpr="3"] .iconfont {
    font-size: 60px;
  }
  [data-dpr="3"] .iconfont.big {
    font-size: 78px;
  }
  body {
    padding-bottom: 1.2rem;
  }
  .title-font {
    font-family: serif;
  }
  .l-m-ellipsis {
    /* autoprefixer: off */
    display: -webkit-box;
    -webkit-box-orient: vertical;
    overflow: hidden;
    text-overflow: ellipsis;
  }
  .l-flex {
    display:flex;
  }
  .g-show-container {
    opacity: 0;
  }
  .g-show {
    transition: opacity 0.3s;
    opacity: 1;
  }
  .g-show-transition {
    transition: all .3s ease;
    opacity: 1;
  }
  .g-show-enter, 
  .g-show-leave {
    opacity: 0;
  }
</style>
