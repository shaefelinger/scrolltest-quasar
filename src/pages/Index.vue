<template>
  <q-page class="flex flex-center  test">
    <img
      alt="Quasar logo"
      src="~assets/quasar-logo-full.svg"
      class="fixed-center light-dimmed"
      style="opacity: 0.2"
    />

    <div class="q-pa-md z-top   ">
      <q-infinite-scroll @load="onLoad" :offset="250">
        <div v-for="(item, index) in items" :key="index" class="text-h4">
          <p>{{ item }}</p>
          <br />
        </div>
      </q-infinite-scroll>
    </div>
  </q-page>
</template>

<script>
export default {
  data() {
    return {
      items: [{}],
      timerId: null
    };
  },

  methods: {
    pageScroll() {
      window.scrollBy(0, 1);
    },
    onLoad(index, done) {
      console.log(index);
      if (this.items) {
        this.items.push("1 - one", "2 - two", "3 - three");
        done();
      }
    }
  },
  mounted() {
    let timerId = setInterval(() => this.pageScroll(), 22);
  },
  destroyed() {
    clearInterval(this.timerId);
  }
};
</script>

<style></style>
