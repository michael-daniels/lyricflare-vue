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
              <i class="fa fa-search" aria-hidden="true"></i>Rhymes
            </div>
            <div class="single-option" @click="toggleShowNextWords">
              <i class="fa fa-arrow-right" aria-hidden="true"></i>Words
            </div>
            <div class="single-option" @click="toggleBank">
              <i class="fas fa-lightbulb"></i>Ideas
            </div>
            <!--div class="single-option" @click="toggleGenre">
              <i class="fa fa-upload" aria-hidden="true"></i>Genre
            </div-->
            <div class="single-option" @click="toggleUploadLyrics">
              <i class="fa fa-upload" aria-hidden="true"></i>Lyrics
            </div>
            <!--div class="single-option" @click="toggleUploadMusic">
              <i class="fa fa-upload" aria-hidden="true"></i>Music
            </div-->
          </div>
          <div class="close-modal" @click="closeModal">
            <i class="fa fa-times" aria-hidden="true"></i>
          </div>
        </div>

        <div class="action-bar" v-if="showRhymes">
          <div class="action-left suggestion-scroll">
            <span v-for="word in song.tempRhymes"><span class="word-suggestion" @click="copyToLocalStorage(word)">{{ word }}</span>   |   </span>
          </div>
          <div class="action-right">
            <i class="fa fa-times fa-times-action" aria-hidden="true" @click="hideAllActions"></i>
          </div>
        </div>

        <div class="action-bar" v-if="showNextWords">
          <div class="action-left suggestion-scroll">
            <span v-for="word in song.tempWords"><span class="word-suggestion">{{ word }}</span>   |   </span>
          </div>
          <div class="action-right">
            <i class="fa fa-times fa-times-action" aria-hidden="true" @click="hideAllActions"></i>
          </div>
        </div>

        <div class="action-bar" v-if="showBank">
          <div class="action-left">
            <span v-for="word in song.tempWords"><span class="word-suggestion">{{ word }}</span>   |   </span>
          </div>
          <div class="action-right">
            <i class="fa fa-times fa-times-action" aria-hidden="true" @click="hideAllActions"></i>
          </div>
        </div>

        <div class="action-bar" v-if="showUploadLyrics">
          <div class="action-left">
            <div class="action-tip">GET PRO Upload lyrics from other songs to mimic their writing style</div>
            <textarea class="upload-lyrics" placeholder="Paste lyrics here" @input="uploadLyricsHandler"></textarea>
          </div>
          <div class="action-right">
            <i class="fa fa-times fa-times-action" aria-hidden="true" @click="hideAllActions"></i>
          </div>
        </div>

        <div class="action-bar" v-if="showUploadMusic">
          <div class="action-left">
            <span class="action-tip">GET PRO Adding music will allow you to play and control it as you write</span>
            <input class="music-url" type="text" placeholder="YouTube link">
          </div>
          <div class="action-right">
            <i class="fa fa-times fa-times-action" aria-hidden="true" @click="hideAllActions"></i>
          </div>
        </div>

        <div class="action-bar" v-if="showGenreMenu">
          <div class="action-left">
            <pre>{{ song.genre }}</pre>
            <span class="action-tip">GET PRO The genre you choose will influence writing assistance decisions</span>
            <select class="select-genre" v-model="song.genre">
              <option value="Pop">Pop</option>
              <option value="Country">Country</option>
              <option value="Hip-hop">Hip-hop</option>
            </select>
          </div>
          <div class="action-right">
            <i class="fa fa-times fa-times-action" aria-hidden="true" @click="hideAllActions"></i>
          </div>
        </div>

        <div class="modal-content">
          <div class="modal-content-item">
              <textarea ref="mainTextArea" class="main-text-area" v-model="song.songLyrics" name="name" placeholder="Your next hit starts here..." @click="selectWord($event)">

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
    song: Object,
  },

  data() {
    return {
      isHover: false,
      showRhymes: false,
      showNextWords: false,
      showUploadLyrics: false,
      showUploadMusic: false,
      showGenreMenu: false,
      showBank: false,
      selectedWord: '',
      confirm: 'Uploaded! Add more...',
    }
  },

  methods: {
    openModal() {
      this.$modal.show(`${this.song.id}`);
    },

    closeModal() {
      this.$modal.hide(`${this.song.id}`);
    },

    checkHover() {
      if (event.type === 'mouseover') {
        this.isHover = true;

      } else if (event.type === 'mouseout') {
        this.isHover = false;
      }
    },

    toggleShowRhymes() {
      this.showRhymes = true;
      this.showBank = false;
      this.showGenreMenu = false;
      this.showUploadMusic = false;
      this.showUploadLyrics = false;
      this.showNextWords = false;

      this.getRhymes(this.selectedWord, this.song.uploadLyrics, this.song.id);
    },

    toggleShowNextWords() {
      this.showBank = false;
      this.showGenreMenu = false;
      this.showUploadMusic = false;
      this.showUploadLyrics = false;
      this.showRhymes = false;
      this.showNextWords = true;

      this.getNextWords(this.selectedWord, this.song.uploadLyrics, this.song.id);
    },

    toggleUploadLyrics() {
      this.showBank = false;
      this.showGenreMenu = false;
      this.showUploadMusic = false;
      this.showUploadLyrics = true;
      this.showRhymes = false;
      this.showNextWords = false;
    },

    toggleUploadMusic() {
      this.showBank = false;
      this.showGenreMenu = false;
      this.showUploadMusic = true;
      this.showUploadLyrics = false;
      this.showRhymes = false;
      this.showNextWords = false;
    },

    toggleGenre() {
      this.showBank = false;
      this.showGenreMenu = true;
      this.showUploadMusic = false;
      this.showUploadLyrics = false;
      this.showRhymes = false;
      this.showNextWords = false;
    },

    toggleBank() {
      this.showBank = true;
      this.showGenreMenu = false;
      this.showUploadMusic = false;
      this.showUploadLyrics = false;
      this.showRhymes = false;
      this.showNextWords = false;
    },

    hideAllActions() {
      this.showBank = false;
      this.showGenreMenu = false;
      this.showUploadMusic = false;
      this.showUploadLyrics = false;
      this.showRhymes = false;
      this.showNextWords = false;
    },

    selectWord(event) {

      if (localStorage.getItem('copiedWord') === 'true') {

        let selectionValue = event.target.selectionStart;

        let songLyricsArray = this.song.songLyrics.split('')

        songLyricsArray[selectionValue] = `${localStorage.getItem('clipboard')} `

        this.song.songLyrics = songLyricsArray.join('')

        //hooking in to this for the insert word feature
          localStorage.setItem('copiedWord', 'false')
          console.log(event)
          console.log(this.song)
          console.log(document.body)

      } else {


        if (event.target.selectionStart === event.target.selectionEnd) {

          let selectionValue = event.target.selectionStart;

          let startIndex = null;
          let endIndex = null;

          let selectedWord = '';

          for (let i = selectionValue; i < event.target.value.length; i++) {
            if (event.target.value[i] === ' ') {
              startIndex = i;
              break;
            }
          }

          for (let i = startIndex - 1; i >= 0; i--) {
            if (event.target.value[i] === ' ') {
              endIndex = i;
              break;
            }
            selectedWord += event.target.value[i]
          }

          this.$refs.mainTextArea.setSelectionRange(endIndex, startIndex)
          this.selectedWord = selectedWord.split('').reverse('').join('');

        } else {
            let inputValue = event.target.value;
            let selectedWord = '';

            for (let i = event.target.selectionStart; i < event.target.selectionEnd; i++) {
              selectedWord += inputValue[i];
            }

            this.selectedWord = selectedWord;
        }


      }
    },

    uploadLyricsHandler() {
      let uploadedLyricsArray = event.target.value.split(' ');
      let uploadLyricsClone = [...this.song.uploadLyrics];
      let newUploadLyrics = uploadLyricsClone.concat(uploadedLyricsArray)

      this.song.uploadLyrics = newUploadLyrics;

      event.target.value = this.confirm;

      if (this.confirm === 'Uploaded! Add more...') {
        this.confirm = 'Got it! Add another...';
      } else {
        this.confirm = 'Uploaded! Add more...'
      }
    },

    getRhymes(userInput, uploadedLyrics, songId) {
      fetch(`https://api.datamuse.com/words?rel_rhy=${userInput}`)
        .then((data) => {
          return data.json();
        })
        .then((rhymes) => {
          if (uploadedLyrics.length > 0) {
            this.song.tempRhymes = [];
            for (let i = 0; i < rhymes.length; i++) {
              if (uploadedLyrics.includes(rhymes[i].word)) {
                this.song.tempRhymes.push(rhymes[i].word)
              }
            }

            if (this.song.tempRhymes.length < 100) {
              for (let i = 0; i < rhymes.length; i++) {
                if (!this.song.tempRhymes.includes(rhymes[i].word)) {
                  this.song.tempRhymes.push(rhymes[i].word)
                }
              }
            }

          } else {
              this.song.tempRhymes = [];
              for (let i = 0; i < rhymes.length; i++) {
                this.song.tempRhymes.push(rhymes[i].word)
              }
          }
        })
        .catch((error) => {
          alert(error)
        })
    },

    getNextWords(userInput, uploadedLyrics, songId) {
      fetch(`https://api.datamuse.com/words?lc=${userInput}`)
        .then((data) => {
          return data.json();
        })
        .then((words) => {
          if (uploadedLyrics.length > 0) {
            this.song.tempWords = [];
            for (let i = 0; i < words.length; i++) {
              if (uploadedLyrics.includes(words[i].word)) {
                this.song.tempWords.push(words[i].word)
              }
            }

            if (this.song.tempWords.length < 100) {
              for (let i = 0; i < words.length; i++) {
                if (!this.song.tempWords.includes(words[i].word)) {
                  this.song.tempWords.push(words[i].word)
                }
              }
            }

          } else {
            this.song.tempWords = [];
            for (let i = 0; i < words.length; i++) {
              this.song.tempWords.push(words[i].word)
            }

          }

        })
        .catch((error) => {
          alert(error)
        })
    },

    copyToLocalStorage(word) {
      localStorage.setItem('clipboard', word)
      localStorage.setItem('copiedWord', 'true')
    },

    insertWord() {
      localStorage.setItem('copiedWord', 'false')
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
    font-family: Bree Serif;
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

  .main-text-area {
    width: 100%;
    min-height: 100vh;
    text-align: center;
    padding: 10vh 0;
    font-size: 26px;
    border: none;
    outline: none;
    color: gray;
    cursor: pointer;
  }

  .main-text-area::selection {
    background-color: limegreen;
    color: white;
  }

  .fa {
    margin-right: 10px;
  }

  .fas {
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
    border: none;
    outline: none;
    border-radius: 5px;
    min-height: 90px;
    padding-top:50px;
    text-align: center;
    margin: 20px 0;
    font-size: 24px;
    color: limegreen;
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
    align-items: top;
    justify-content: center;
  }

  .action-tip {
    font-family: Bree Serif;
    color: white;
  }

  .select-genre {
    min-width: 10vw;
    margin: 0 50px;
    border: none;
    font-size: 16px;
    text-align: center;
    font-family: Bree Serif;
  }

  .music-url {
    height: 20px;
    width: 150px;
    margin: 0 50px;
    text-align: center;
    outline: none;
    border-radius: 5px;
    border: none;
  }

  .suggestion-scroll {
    white-space: nowrap;
    overflow: scroll;
    scroll: hidden;
  }

</style>
