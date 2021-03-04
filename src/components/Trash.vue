<template>
  <v-row class="my-9">
    <v-spacer />
    <v-col
      cols="12"
      sm="6"
      md="4"
      style="text-align:center"
      @dragover.prevent
      @drop="dropOnTrash"
    >
      <v-card class="text-center:my-10" style="border:outset 5px red">
        <span style="color:#CC0033; font-size:20px">
          Drag & Drop Movie's Comment Here!
        </span>
        <v-icon color="red" style="font-size:50px">
          mdi-trash-can
        </v-icon>
      </v-card>
    </v-col>
  </v-row>
</template>

<script>
export default {
  props: {
    droppedMovieId: {
      type: Number,
      default: 0,
    },
  },

  methods: {
    // 動画削除処理
    dropOnTrash() {
      this.$emit("deleteMovie", this.droppedMovieId);
    },
  },
};
</script>
