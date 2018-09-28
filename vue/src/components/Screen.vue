<template>
  <div class="screen">
    <navbar :state="state">Hey there</navbar>
    <div class="main-content">

        <div class="heading">
          <h1>My Songs</h1>
        </div>

        <div class="song-tile-container">
          <div class="ghost-song-tile">
            +
          </div>

          <song-tile class="song-tile" v-for="song in state.savedSongs" :song="song" />
        </div>

        <img class="banner-ad" src="http://www.effectsbay.com/wp-content/uploads/2011/07/guitar-center-10-percent-10days.jpg" alt="">

    </div>
  </div>
</template>

<script>
import Navbar from './Navbar.vue'
import SongTile from './SongTile.vue'

export default {
  components: {
    Navbar,
    SongTile
  },

  name: 'Screen',

  props: {
    state: Object
  },

  data() {
    return {

    }
  },
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>

  h1 {
    margin: 25px;
  }
  .main-content {
    height: 100vh;
    padding: 19vh 7vw;
    background-color: #f2f2f2;
    text-align: center;
  }

  .ghost-song-tile {
      min-width: 290px;
      max-width: 290px;
      max-height: 290px;
      background-color: transparent;
      border-radius: 50%;
      padding: 110px 0;
      align-items: center;
      margin: 25px;
      text-align: center;
      border: 5px limegreen dashed;
      font-size: 50px;
      color: limegreen;
    }

    .ghost-song-tile:hover {
        min-width: 290px;
        max-width: 290px;
        max-height: 290px;
        background-color: transparent;
        box-shadow: 1px 1px 1px 1px limegreen;
        border-radius: 50%;
        padding: 110px 0;
        align-items: center;
        margin: 25px;
        text-align: center;
        border: 5px limegreen dashed;
        font-size: 50px;
        color: limegreen;
      }

    .song-tile-container {
      display: flex;
      overflow: scroll;
    }

    .banner-ad {
      margin: 25px 0;
    }

</style>
