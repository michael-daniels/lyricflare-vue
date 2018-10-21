<template>
  <div id="app">
    <screen :state="state"/>
  </div>
</template>

<script>
import Screen from './components/Screen.vue'

export default {
  name: 'app',
  components: {
    Screen
  },

  data() {
    return {
      state: {
        siteName: 'SmartSong',
        logo: 'https://mbtskoudsalg.com/images/fire-logo-png-2.png',
        loggedIn: true,
        isAdmin: true,
        isOwner: true,
        lastLogin: 'yesterday',
        currentUser: {
          firstName: 'Michael',
          lastName: 'Daniels',
          age: 33,
          location: 'Phoenix, AZ',
          favoriteGenres: ['Rap', 'Hip-hop', 'Country'],
        },
        timeZone: 'Mountain',
        location: 'some coordinates',
        collaborators: [{ id: 2, firstName: 'Liz', lastName: 'Daniels' }],
        savedSongs: [
          {
            id: 1,
            title: 'Make You Miss Me',
            createdOn: 'somedate',
            lastEdited: 'someotherdate',
            isPublic: false,
            showAds: true,
            songLyrics: '',
            savedInstrumentals: [{ title: 'Drop da Bass' }],
            savedRecordings: [{ title: 'Sample Melody' }],
            savedLyrics: [],
            uploadLyrics: [],
            tempRhymes: [],
            tempWords: [],
            getRhymes: this.getRhymes,
            getNextWords: this.getNextWords,
            genre: 'Country'
          },
          {
            id: 2,
            title: 'Killshot',
            createdOn: 'somedate',
            lastEdited: 'someotherdate',
            isPublic: true,
            songLyrics: '',
            savedInstrumentals: [{ title: 'Get at Em' }],
            savedRecordings: [{ title: 'Flow 2' }],
            savedLyrics: [],
            uploadLyrics: [],
            tempRhymes: [],
            tempWords: [],
            getRhymes: this.getRhymes,
            getNextWords: this.getNextWords,
            genre: 'Pop'
          },
          {
            id: 3,
            title: 'My Kind of Girl',
            createdOn: 'somedate',
            lastEdited: 'someotherdate',
            isPublic: true,
            songLyrics: '',
            savedInstrumentals: [{ title: 'Get at Em' }],
            savedRecordings: [{ title: 'Flow 2' }],
            savedLyrics: [],
            uploadLyrics: [],
            tempRhymes: [],
            tempWords: [],
            getRhymes: this.getRhymes,
            getNextWords: this.getNextWords,
            genre: 'Hip-hop'
          },
          {
            id: 4,
            title: 'Over There',
            createdOn: 'somedate',
            lastEdited: 'someotherdate',
            isPublic: true,
            songLyrics: '',
            savedInstrumentals: [{ title: 'Get at Em' }],
            savedRecordings: [{ title: 'Flow 2' }],
            savedLyrics: [],
            uploadLyrics: [],
            tempRhymes: [],
            tempWords: [],
            getRhymes: this.getRhymes,
            getNextWords: this.getNextWords,
            genre: 'Country'
          },
        ],
      },
    }
  },

  watch: {
    'state.savedSongs': {
      handler(newVal, oldVal) {
        console.log('dis it', this.state.savedSongs[0].uploadLyrics)
          // do something with the object
      },
      deep: true,
      }
  },

  methods: {
    getRhymes(userInput, uploadedLyrics, songId) {
      fetch(`https://api.datamuse.com/words?rel_rhy=${userInput}`)
        .then((data) => {
          return data.json();
        })
        .then((rhymes) => {
          for (let i = 0; i < rhymes.length; i++) {
            if (uploadedLyrics.includes(rhymes[i].word)) {
              this.state.savedSongs.find((song) => song.id === songId).tempRhymes.push(rhymes[i].word)
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
            console.log('words if', words)
            for (let i = 0; i < words.length; i++) {
              if (uploadedLyrics.includes(words[i].word)) {
                this.state.savedSongs.find((song) => song.id === songId).tempWords.push(words[i].word)
              }
            }
          } else {
            console.log('words else', words)
            for (let i = 0; i < words.length; i++) {
              this.state.savedSongs.find((song) => song.id === songId).tempWords.push(words[i].word)
            }

          }

        })
        .catch((error) => {
          alert(error)
        })
    }
  },
}
</script>

<style>
  ::placeholder { /* Chrome, Firefox, Opera, Safari 10.1+ */
      color: lightgray;
      opacity: 1; /* Firefox */
  }
  @import url('https://fonts.googleapis.com/css?family=Poppins');
  @import url('https://fonts.googleapis.com/css?family=Bree+Serif');

  * {
    margin: 0;
    padding: 0;
  }

  body {
    background-color: #f2f2f2;
    font-family: Bree Serif;
  }

  h1 {
    color: limegreen;
    font-family: Poppins;
  }

  h2 {
    color: limegreen;
    font-family: Poppins;
  }

  h3 {
    color: gray;
  }

  h4 {
    color: gray;
  }

  h5 {
    color: gray;
  }

  hr {
    max-width: 80%;
    margin: 10px auto;
  }
</style>
