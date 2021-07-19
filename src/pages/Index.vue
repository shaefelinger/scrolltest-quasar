<template>
  <q-page class="flex  flex-center test">
    <div class="fixed-top-left">
      <div class="">
        <q-btn @click="pause">pause</q-btn>
        <q-btn @click="resume">play</q-btn>
      </div>
    </div>

    <div class="q-pa-md z-top  scroll-container " ref="scrollContainer">
      <q-infinite-scroll
        class="infiniteScroll"
        @load="triggerInfiniteScroll"
        :offset="10"
        debounce="0"
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

gsap.registerPlugin(ScrollToPlugin);

let masterTL = gsap.timeline();

export default {
  data() {
    return {
      items: [],
      episodes: ["1 - one", "2 - two", "3 - three", "---"],
      // episodeCount: 0,
      isScrolling: false
    };
  },

  methods: {
    loadNewData() {
      for (let i = 0; i < 5; i++) {
        this.items = this.items.concat(this.episodes);
      }
      console.log("load new data", this.items.length);
    },
    triggerInfiniteScroll(index, done) {
      console.log("triggerInfiniteScroll");
      this.loadNewData();
      this.addTween();
      this.play();
      console.log(gsap.getTweensOf(window));

      // this.episodeCount++;
      // if (this.episodeCount > 3) {
      //   this.resetAll();
      // }
      done();
    },
    // resetAll() {
    // },
    gaspAutoscroll() {
      let tl = gsap.timeline().to(window, {
        duration: 14,
        ease: "linear",
        scrollTo: {
          y: "max"
          // autoKill: true
        }
      });
      return tl;
    },
    addTween() {
      masterTL.add(this.gaspAutoscroll());
    },
    play() {
      masterTL.play();
    },
    resume() {
      masterTL.paused(false);
    },
    pause() {
      console.log("pause");
      masterTL.pause();
    },
    userScrolls(event) {
      console.log("user scrolls", event.deltaY);
      // while the user is scrolling:
      window.clearTimeout(this.isScrolling);
      // Set a timeout to run after scrolling ends
      this.isScrolling = setTimeout(() => {
        const yPosition = window.scrollY;
        console.log("scroll position:", yPosition);
        console.log("Scrolling has stopped.");
        masterTL.play();
      }, 500);
      this.pause();
    }
  },
  mounted() {
    this.loadNewData();
    this.addTween();
    this.play();
    window.addEventListener("wheel", this.userScrolls, {
      passive: true
    });
  }
};
</script>

<style>
.scroll-container {
  background: lightgrey;
  width: 600px;
}
</style>
