<template>
  <q-page class="flex  flex-center test">
    <div class="fixed-top-left">
      <div class="">
        <q-btn @click="pause">pause</q-btn>
        <q-btn @click="resume">play</q-btn>
        <p>{{ isScrolling }}</p>
      </div>
    </div>

    <div class="q-pa-md z-top  scroll-container " ref="scrollContainer">
      <q-infinite-scroll
        class="infiniteScroll"
        @load="loadAutoscroll"
        :offset="10"
        debounce="0"
        v-on:scroll.passive="userScrolls"
      >
        <div v-for="(item, index) in items" :key="index" class="text-h4">
          <p>{{ item }}- {{ index }}</p>
        </div>
      </q-infinite-scroll>
    </div>
  </q-page>
</template>

<script>
import gsap from "gsap";
import { ScrollToPlugin } from "gsap/ScrollToPlugin";
// import { ScrollTrigger } from "gsap/ScrollTrigger";

gsap.registerPlugin(ScrollToPlugin);
// gsap.registerPlugin(ScrollTrigger);

let masterTL = gsap.timeline();

export default {
  data() {
    return {
      items: [],
      episodes: ["1 - one", "2 - two", "3 - three", "---"],
      episodeCount: 0,
      isScrolling: false
    };
  },

  methods: {
    addTween() {
      masterTL.add(this.gaspAutoscroll());
    },
    play() {
      masterTL.play();
      // this.gaspAutoscroll();
    },
    resume() {
      masterTL.paused(false);
    },
    pause() {
      // console.log("pause");
      // console.log(masterTL.progress());
      // masterTL.progress(1);
      masterTL.pause();
      // this.gsapAutoscroll.pause();
      // masterTL.reverse();
    },

    userScrolls(event) {
      console.log("user scrolls", event.deltaY);
      //
      //  Clear our timeout throughout the scroll
      window.clearTimeout(this.isScrolling);
      //
      // Set a timeout to run after scrolling ends
      this.isScrolling = setTimeout(() => {
        const yPosition = window.scrollY;
        console.log("scroll position:", yPosition);
        // Run the callback
        console.log("Scrolling has stopped.");
        // gsap.to(window, { scrollTo: { y: yPosition } });
        masterTL.play();
        // this.resume();
      }, 500);
      this.pause();
      //
      // window.scrollBy(0, event.deltaY);
      // masterTL.play();
      // this.anmiation.pause();
    },
    gaspAutoscroll() {
      let tl = gsap.timeline().to(window, {
        duration: 3,
        scrollTo: {
          y: "max"
          // autoKill: true
          // onAutoKill: this.myAutoKillFunction
        },
        ease: "linear"
        // onUpdate: this.update
      });
      return tl;
    },
    // update() {
    //   console.log(masterTL.progress().toFixed(2));
    // },
    myAutoKillFunction() {
      console.log("autokill");
      // setTimeout(this.play, 500);
      // this.gaspAutoscroll();
      // alert("autoKill");
    },
    loadAutoscroll(index, done) {
      console.log("infiniteScroll trigger");
      this.loadNewData();

      console.log(this.items.length);
      // this.gaspAutoscroll();
      this.addTween();
      this.play();
      console.log(gsap.getTweensOf(window));

      this.episodeCount++;
      // if (this.episodeCount > 3) {
      //   this.resetAll();
      // }
      done();
    },
    resetAll() {
      console.log("clear");
      this.items = [];
      this.episodeCount = 0;
      // this.loadNewData();
      // this.addTween();
      // this.play();
    },
    loadNewData() {
      console.log("load new data");
      // this.items = this.items.concat(this.episodes);
      for (let i = 0; i < 5; i++) {
        this.items = this.items.concat(this.episodes);
      }
    }
    // pageScroll() {
    //   window.scrollBy(0, 1);
    // }
  },
  mounted() {
    // gsap.config({ autoKillThreshold: 1 });

    this.loadNewData();
    // this.gaspAutoscroll();
    this.addTween();

    this.play();

    // window.onwheel = this.userScrolls;
    window.addEventListener("wheel", this.userScrolls, {
      passive: true
    });

    // let timerId = setInterval(() => this.pageScroll(), 22);

    // gsap.to(window, { duration: 2, scrollTo: { y: "max" }, ease: "linear" });
  },
  destroyed() {
    // clearInterval(this.timerId);
  }
};
</script>

<style>
.scroll-container {
  background: red;
  width: 600px;
  /* height: 400px;
  overflow: scroll; */
}
</style>
