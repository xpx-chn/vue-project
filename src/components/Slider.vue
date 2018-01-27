<template>
  <div class="slide-show" @mouseenter="stopRun" @mouseleave="startRun">
    <div class="slide-img">
      <a :href="slides[curIndex].href">
        <transition name="trans-slide">
          <img v-if="curShow" :src="slides[curIndex].src">
        </transition>
        <transition name="trans-slide-old">
          <img v-if="!curShow" :src="slides[curIndex].src">
        </transition>
      </a>
    </div>
    <h2>{{slides[curIndex].title}}</h2>
    <ul class="slide-pages">
      <li @click="goto(prevIndex)">&lt;</li>
      <li class="page-index" v-for="(item, index) in slides" :key="item.title"
       @click="goto(index)">
        <a :class="{active: curIndex === index}" href="javascript:;">{{index+1}}</a>
      </li>
      <li @click="goto(nextIndex)">&gt;</li>
    </ul>
  </div>
</template>
<script>
export default {
  name: "slider",
  created() {
    console.log(this.slides);
  },
  props: {
    slides: {
      type: Array,
      default: []
    },
    duration: {
      type: Number,
      default: 3000
    }
  },
  data() {
    return {
      curIndex: 0,
      curInterval: null,
      slideLen: this.slides.length,
      curShow: true
    };
  },
  computed: {
    prevIndex() {
      var prevIndex = this.curIndex - 1;
      return prevIndex < 0 ? this.slideLen - 1 : prevIndex;
    },
    nextIndex() {
      var nextIndex = this.curIndex + 1;
      return nextIndex >= this.slideLen ? 0 : nextIndex;
    }
  },
  methods: {
    goto(index) {
      this.curShow = false;
      setTimeout(() => {
        this.curShow = true;
        this.curIndex = index;
      });
    },
    startRun() {
      this.curInterval = setInterval(() => {
        this.goto(this.nextIndex);
      }, this.duration);
    },
    stopRun() {
      this.curInterval && clearInterval(this.curInterval);
    }
  },
  mounted() {
    this.startRun();
  }
};
</script>
<style>

.trans-slide-enter-active,
.trans-slide-old-leave-active
{
  transition: all .5s;
}

.trans-slide-enter{
  transform: translateX(900px);
}
.trans-slide-old-leave-active {
  transform: translateX(-900px);
}

.slide-show {
  position: relative;
  margin: 15px 15px 15px 0;
  width: 900px;
  height: 500px;
  overflow: hidden;
}
.slide-show h2 {
  position: absolute;
  width: 100%;
  height: 100%;
  color: #fff;
  background: #000;
  opacity: 0.5;
  bottom: 0;
  height: 30px;
  line-height: 25px;
  text-align: left;
  padding-left: 15px;
}
.slide-img {
  width: 100%;
  height: 100%;
  position: relative;
}
.slide-img a {
  position: absolute;
  top: 0;
  bottom: 0;
  left: 0;
  right: 0;
  cursor: pointer;
}
.slide-img img {
  width: 100%;
  position: absolute;
  top: 0;
}
.slide-pages {
  position: absolute;
  bottom: 10px;
  right: 15px;
}
.slide-pages li {
  display: inline-block;
  padding: 0 10px;
  cursor: pointer;
  color: #fff;
}
.page-index a.active {
  color: #0bc489;
}
.slide-pages li .on {
  text-decoration: underline;
}
</style>
