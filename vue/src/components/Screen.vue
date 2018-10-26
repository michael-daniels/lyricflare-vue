<template>
  <div class="screen">
    <navbar :state="state">Hey there</navbar>
    <div class="main-content">

        <div class="heading">
          <h1>My Songs</h1>
        </div>

        <div class="song-tile-container">
          <div v-if="!isCreating" class="ghost-song-tile" @click="toggleIsCreating">
            +
          </div>
          <div v-else class="ghost-song-tile">
            <form class="" @submit="addNewSong">
              <input type="text" ref="titleInput" v-model="state.newSong.title" placeholder="New song title" class="new-song-title-input" @mouseout="toggleIsCreating">
            </form>

          </div>

          <song-tile class="song-tile" v-for="song in state.savedSongs" :song="song" :tempRhymes="state.tempRhymes" :tempWords="state.tempWords" @emitDeleteSong="deleteSong" />

        </div>



    </div>

    <div class="footer">
      <div>
        LyricFlare™, © 2018
      </div>
    </div>
    <footer></footer>
  </div>
</template>

<script>
import Vue from 'vue'
import Navbar from './Navbar.vue'
import SongTile from './SongTile.vue'
import Footer from './Footer.vue'

export default {
  components: {
    Navbar,
    SongTile,
    Footer,
  },

  name: 'Screen',

  props: {
    state: Object
  },

  data() {
    return {
      isCreating: false,
    }
  },

  methods: {
    toggleIsCreating() {
      this.isCreating = !this.isCreating
    },

    addNewSong() {
      event.preventDefault();
      this.state.newSong.id = this.state.savedSongs.length + 1;
      this.state.savedSongs.unshift({...this.state.newSong});
      this.state.newSong.title = '';
    },

    deleteSong(value) {
      console.log('DELETE SONG HEARD IN PARENT', value)
      console.log('state', this.state)
      this.state.savedSongs = this.state.savedSongs.filter((song) => {
        return song !== value
      })
    },
  },
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>

  h1 {
    font-size: 2em;
    margin: 25px;
  }
  .main-content {
    min-height: 100vh;
    padding: 10vh 7vw;
    background-color: #f2f2f2;
    text-align: center;
  }

  .ghost-song-tile {
    display: flex;
    align-items: center;
    justify-content: center;
    max-height: 270px;
    max-width: 270px;
    min-width: 270px;
    min-width: 270px;
    height: 270px;
    width: 270px;
    background-color: transparent;
    border-radius: 10px;
    margin: 25px;
    border: 3px limegreen dashed;
    font-size: 50px;
    color: limegreen;
    cursor: pointer;
  }

    .ghost-song-tile:hover {
        background-color: limegreen;
        color: white;
      }

    .song-tile-container {
      display: flex;
      flex-direction: row;
      flex-flow: row wrap;
      justify-content: center;
    }

    .banner-ad {
      margin: 25px 0;
    }

    .new-song-title-input {
      text-align: center;
      height: 45px;
      width: 80%;
      font-size: 24px;
      border-radius: 5px;
      outline: none;
      border: none;
      color: gray;
    }

    .footer {
      color: white;
      font-family: Verdana;
      background-color: limegreen;
      min-height: 70px;
      padding: 0 75px;
      display: flex;
      align-items: center;
    }

    @import url('https://use.fontawesome.com/releases/v5.2.0/css/all.css');
    @import url('http://cdn.materialdesignicons.com/2.5.94/css/materialdesignicons.min.css');

</style>
