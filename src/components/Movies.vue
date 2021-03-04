<template>
  <div>
    <v-row>
      <template v-if="loading">
        <v-container class="px-10 py-10" style="text-align:center">
          <v-progress-circular size="70" color="blue" indeterminate />
        </v-container>
      </template>
      <template v-else>
        <v-col
          v-for="item in movieInternalItems"
          :key="item.id"
          cols="12"
          sm="6"
          md="4"
          class="pr-0"
          style="width:290px"
        >
          <div style="text-align:center">
            <iframe
              :src="item.url"
              width="290"
              height="163.125"
              frameborder="0"
            />
            <v-card
              draggable
              @dragstart="dragMovie(item.id)"
              color="blue"
              height="50"
              width="290"
              style="margin:auto"
            >
              <v-col>
                <p style="color:white">
                  {{ item.comment }}
                </p>
              </v-col>
            </v-card>
          </div>
        </v-col>
      </template>
    </v-row>
    <trash :dropped-movie-id="droppedMovieId" @deleteMovie="deleteMovie" />
  </div>
</template>

<script>
import Trash from "./Trash.vue";
export default {
  props: {
    movieItems: {
      type: Array,
      default: null,
    },
    loading: {
      type: Boolean,
      required: true,
      default: true,
    },
  },
  components: {
    Trash,
  },
  data() {
    return {
      movieInternalItems: [],
      droppedMovieId: 0,
    };
  },

  methods: {
    init() {
      this.movieInternalItems = [];
      Object.keys(this.movieItems).forEach((i) => {
        const newItem = {
          id: this.movieItems[i].id,
          url:
            "https://www.youtube.com/embed/" +
            this.movieItems[i].url +
            "?controls=1&loop=1&playlist=" +
            this.movieItems[i].url,
          comment: this.movieItems[i].comment,
        };
        this.movieInternalItems.push(newItem);
      });
      this.movieInternalItems.sort(this.descending);
    },
    descending(a, b) {
      let comparison = 0;
      if (a.id > b.id) {
        comparison = -1;
      } else if (b.id > a.id) {
        comparison = 1;
      }
      return comparison;
    },
    dragMovie(id) {
      this.droppedMovieId = id;
    },
    deleteMovie(id) {
      this.$emit("deleteMovie", id);
    },
  },
};
</script>
