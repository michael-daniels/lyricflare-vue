<template>
  <div class="song-tile" @click="openModal" @mouseover="checkHover" @mouseout="checkHover">
      <div class="title">
        <h2>{{ song.title }}</h2>
      </div>
      <hr />
      <div class="created-on">
        <h5>Created: {{ song.createdOn }}</h5>
      </div>
      <div class="last-edited">
        <h5>Last edited: {{ song.lastEdited }}</h5>
      </div>

      <modal class="modal" :name="song.id.toString()" :draggable="false" :adaptive="true" :width="1200" :height="600">
        <div class="modal-header">
          <div class="song-title">
            <h1>{{ this.song.title }}</h1>
          </div>
          <div class="modal-options">
            <div class="single-option" @click="toggleShowRhymes">
              <i class="fa fa-search" aria-hidden="true"></i>Find Rhyme
            </div>
            <div class="single-option" @click="toggleShowNextWords">
              <i class="fa fa-arrow-right" aria-hidden="true"></i>Next Word
            </div>
            <div class="single-option" @click="toggleUploadLyrics">
              <i class="fa fa-upload" aria-hidden="true"></i>Upload Lyrics
            </div>
          </div>
          <div class="close-modal" @click="closeModal">
            <i class="fa fa-times" aria-hidden="true"></i>
          </div>
        </div>

        <div class="action-bar" v-if="showRhymes">
          <div class="action-left">
            <span v-for="word in song.tempRhymes"><span class="word-suggestion">{{ word }}</span>   |   </span>
          </div>
          <div class="action-right">
            <i class="fa fa-times fa-times-action" aria-hidden="true" @click="hideAllActions"></i>
          </div>
        </div>

        <div class="action-bar" v-if="showNextWords">
          <div class="action-left">
            <span v-for="word in song.tempNextWords"><span class="word-suggestion">{{ word }}</span>   |   </span>
          </div>
          <div class="action-right">
            <i class="fa fa-times fa-times-action" aria-hidden="true" @click="hideAllActions"></i>
          </div>
        </div>

        <div class="action-bar" v-if="showUploadLyrics">

          <div class="action-left">
            <textarea class="upload-lyrics" placeholder="Paste lyrics here"></textarea>
          </div>
          <div class="action-right">
            <i class="fa fa-times fa-times-action" aria-hidden="true" @click="hideAllActions"></i>
          </div>
        </div>

        <div class="modal-content">
          <div class="modal-content-item">
              <textarea class="text-area" v-model="song.savedLyrics" name="name" placeholder="Your next hit starts here...">

              </textarea>
          </div>
        </div>
      </modal>
  </div>
</template>

<script>
import Vue from 'vue'
import vmodal from 'vue-js-modal'

export default {
  name: 'SongTile',

  props: {
    song: Object
  },

  data() {
    return {
      isHover: false,
      showRhymes: false,
      showNextWords: false,
      showUploadLyrics: false,
    }
  },

  methods: {
    openModal() {
      console.log('open modal', this.song.id)
      this.$modal.show(`${this.song.id}`);
    },

    closeModal() {
      console.log('close modal', this.song.id)
      this.$modal.hide(`${this.song.id}`);
      console.log(this.song.savedLyrics);
    },

    checkHover() {
      if (event.type === 'mouseover') {
        this.isHover = true;

      } else if (event.type === 'mouseout') {
        this.isHover = false;
      }
    },

    toggleShowRhymes() {
      this.showUploadLyrics = false;
      this.showRhymes = true;
      this.showNextWords = false;
    },

    toggleShowNextWords() {
      this.showUploadLyrics = false;
      this.showRhymes = false;
      this.showNextWords = true;
    },

    toggleUploadLyrics() {
      this.showUploadLyrics = true;
      this.showRhymes = false;
      this.showNextWords = false;
    },

    hideAllActions() {
      this.showUploadLyrics = false;
      this.showRhymes = false;
      this.showNextWords = false;
    },
  },

}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
  .song-tile {
    min-width: 300px;
    max-width: 300px;
    max-height: 300px;
    padding: 110px 0;
    background-color: white;
    box-shadow: 1px 1px 1px 1px silver;
    border-radius: 50%;
    flex: 1;
    align-items: center;
    margin: 25px;
  }

  .song-tile:hover {
    min-width: 300px;
    max-width: 300px;
    max-height: 300px;
    padding: 110px 0;
    background-color: white;
    box-shadow: 1px 1px 1px 1px limegreen;
    border-radius: 50%;
    flex: 1;
    align-items: center;
    margin: 25px;
    cursor: pointer;
  }

  .created-on {

  }

  .last-edited {

  }

  .modal {
    align-items: center;
  }

  .modal-header {
    display: flex;
  }

  .close-modal {
    flex: 1;
    display: flex;
    align-items: center;
    justify-content: center;
    color: limegreen;
  }

  .song-title {
    flex: 2;
    text-align: center;
    color: limegreen;
    margin: 25px;
  }

  .modal-options {
    flex: 3;
    align-items: center;
    justify-content: center;
    display: flex;
  }

  .single-option {
    flex: 1;
    display: flex;
    align-items: center;
    justify-content: center;
    color: lightgray;
    font-family: K2D;
    font-size: 16px;
  }

  .single-option:hover {
    color: limegreen;
  }

  .modal-content {
    display: flex;
    align-items: center;
  }

  .modal-content-item {
    width: 100%;
  }

  .text-area {
    width: 100%;
    min-height: 100vh;
    text-align: center;
    padding: 10vh 0;
    font-size: 20px;
    border: none;
    outline: none;
    color: gray;
  }

  .fa {
    margin-right: 10px;
  }

  .fa-times {
    font-size: 24px;
    color: lightgray;
  }

  .fa-times:hover {
    color: limegreen;
  }

  .action-bar {
    display: flex;
    padding: 20px 7vw;
    background-color: limegreen;
  }

  .word-suggestion {
    color: white;
    font-size: 24px;
  }

  .word-suggestion:hover {
    color: white;
  }

  .upload-lyrics {
    width: 100%;
  }

  .fa-times-action {
    color: white;
  }

  .fa-times-action:hover {
    color: white;
  }

  .action-left {
    flex: 10;
  }

  .action-right {
    flex: 1;
    display: flex;
    align-items: center;
    justify-content: center;
  }
</style>
