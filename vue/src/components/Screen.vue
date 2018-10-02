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
              <input type="text" ref="titleInput" v-model="newSong.title" placeholder="Title" class="new-song-title-input" @mouseout="toggleIsCreating">
            </form>

          </div>

          <song-tile class="song-tile" v-for="song in state.savedSongs" :song="song" :tempRhymes="state.tempRhymes" />

        </div>

        <img class="banner-ad" src="http://www.effectsbay.com/wp-content/uploads/2011/07/guitar-center-10-percent-10days.jpg" alt="">

    </div>
  </div>
</template>

<script>
import Vue from 'vue'
import Navbar from './Navbar.vue'
import SongTile from './SongTile.vue'

export default {
  components: {
    Navbar,
    SongTile,
  },

  name: 'Screen',

  props: {
    state: Object
  },

  data() {
    return {
      isCreating: false,
      newSong: {
        id: null,
        title: '',
        createdOn: 'somedate',
        lastEdited: 'someotherdate',
        isPublic: false,
        showAds: true,
        savedInstrumentals: [{ title: 'Drop da Bass' }],
        savedRecordings: [{ title: 'Sample Melody' }],
        savedLyrics: null,
        tempNextWords: [],
        tempRhymes: [],
      },
    }
  },

  methods: {
    toggleIsCreating() {
      this.isCreating = !this.isCreating
    },

    addNewSong() {
      event.preventDefault();
      this.newSong.id = this.state.savedSongs.length + 1;
      this.state.savedSongs.unshift({...this.newSong});
      this.newSong.title = '';
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
      border: 3px limegreen dashed;
      font-size: 50px;
      color: limegreen;
      cursor: pointer;
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
        border: 3px limegreen dashed;
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

    .new-song-title-input {
      text-align: center;
      height: 45px;
      width: 80%;
      font-size: 24px;
      border: 3px limegreen dashed;
      border-radius: 5px;
      outline: none;
    }


    @import url('https://use.fontawesome.com/releases/v5.2.0/css/all.css');
    @import url('http://cdn.materialdesignicons.com/2.5.94/css/materialdesignicons.min.css');

</style>
