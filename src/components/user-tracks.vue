<template>
  <infinite-scroll
    class="user-tracks"
    :items="tracks"
    :total="count"
    v-on="$listeners"
  >
    <v-sheet
      v-for="(item, idx) of tracks"
      :key="item.added"
      dark
      :color="idx % 2 === 0 ? 'accent' : 'secondary'"
      @click="$emit('select', item)"
      class="track d-flex justify-start"
    >
      <v-img
        :src="item.track.album.images[2].url"
        contain
        :max-height="60"
        :max-width="60"
      ></v-img>
      <div class="d-flex flex-column align-self-center ml-2">
        <div>{{ item.track.name }}</div>
        <div>{{ item.track.artists.map((a) => a.name).join(", ") }}</div>
      </div>
    </v-sheet>
  </infinite-scroll>
</template>

<style lang="scss">
.user-tracks {
  .track {
    cursor: pointer;
  }
}
</style>
<script>
import { debounce } from "lodash";
import InfiniteScroll from "./layout/infinite-scroll";

export default {
  components: { InfiniteScroll },
  props: {
    loading: Boolean,
    tracks: Array,
    count: Number,
  },
  data: () => ({
    page: 1,
  }),
  methods: {
    loadMore: debounce(function (entries, observer, isIntersecting) {
      if (!isIntersecting) return;
      this.$emit("page", ++this.page);
    }, 500),
  },
};
</script>