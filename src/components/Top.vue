<template>
  <v-container>
    <inputForm
      formRounded="xl"
      :formElevation="formElevation"
      @storeMovie="storeMovie"
    />
    <v-messages
      :value="responseError"
      color="red"
      class="response-error my-5 text-center"
    />
    <movies
      ref="movies"
      :movie-items="movieItems"
      :loading="loading"
      @deleteMovie="deleteMovie"
    />
  </v-container>
</template>

<script>
import InputForm from "./InputForm";
import Movies from "./Movies.vue";
import axios from "axios";
export default {
  components: {
    InputForm,
    Movies,
  },
  data() {
    return {
      formElevation: "10",
      movieItems: [{}],
      responseError: [],
      loading: true,
    };
  },
  created() {
    this.getMovies();
  },
  methods: {
    getMovies() {
      axios
        .get("https://youtube-curation.herokuapp.com/rest/1")
        .then((response) => {
          console.log(response.data.user.movies);
          this.movieItems = response.data.user.movies;
          setTimeout(() => {
            this.loading = false;
          }, 1000);
        })
        .catch((error) => {
          console.log(error);
          this.responseError = ["動画の取得に失敗しました"];
        })
        .finally(() => {
          this.$refs.movies.init();
        });
    },
    storeMovie(movieUrl, comment) {
      this.loading = true;
      this.responseError = [];
      axios
        .post("https://youtube-curation.herokuapp.com/rest", {
          url: movieUrl,
          comment: comment,
        })
        .then((response) => {
          this.movieItems = response.data.movies;
          setTimeout(() => {
            this.loading = false;
          }, 1000);
        })
        .catch((error) => {
          console.log(error);
          this.responseError = ["動画の投稿に失敗しました"];
        })
        .finally(() => {
          this.$refs.movies.init();
        });
    },
    deleteMovie(id) {
      this.loading = true;
      this.responseError = [];
      axios
        .delete("https://youtube-curation.herokuapp.com/rest/" + id)
        .then((response) => {
          console.log(response.data.movies);
          this.movieItems = response.data.movies;
          setTimeout(() => {
            this.loading = false;
          }, 1000);
        })
        .catch((error) => {
          console.log(error);
          this.responseError = ["動画の削除に失敗しました"];
        })
        .finally(() => {
          this.$refs.movies.init();
        });
    },
  },
};
</script>
<style>
.response-error .v-messages__message {
  font-size: 18px;
}
</style>
