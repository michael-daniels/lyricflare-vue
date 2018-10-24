<template>
  <div id="app">
    <screen :state="state"/>
  </div>
</template>

<script>
import Screen from './components/Screen.vue'
import cloneDeep from 'clone-deep'

export default {
  name: 'app',
  components: {
    Screen
  },

  beforeMount() {
    clearLocalStorage();

    if (localStorage.getItem('lyricFlareState')) {
      this.state = JSON.parse(localStorage.getItem('lyricFlareState'))
    }

    if (!localStorage.getItem('lyricFlareState')) {
      localStorage.setItem('lyricFlareState', JSON.stringify(this.state))
    }
  },

  watch: {
    'state.savedSongs': {
      handler(newSavedSongs, oldSavedSongs) {
          let stateClone = cloneDeep(this.state)
          stateClone.savedSongs = newSavedSongs
          localStorage.setItem('lyricFlareState', JSON.stringify(stateClone))
      },
      deep: true,
      }
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
            title: 'Tutorial Song',
            createdOn: 'somedate',
            lastEdited: 'someotherdate',
            isPublic: false,
            showAds: true,
            songLyrics: `To select a word here, click it. Youll see it highlighted green. \n
              Next, you can click Rhymes, Words, or Ideas above. These are called actions. \n
              Actions return a list of words that go with the selected word,
              depending on what the action name is. \n
              To insert a word from the action bar into your song,
              just click the word you want, then click wherever you want to insert it.\n
              To create a new song, close this one and click the dotted circle, then enter a title.`,
            savedInstrumentals: [{ title: 'Drop da Bass' }],
            savedRecordings: [{ title: 'Sample Melody' }],
            savedLyrics: [],
            uploadLyrics: [],
            tempRhymes: [],
            tempWords: [],
            genre: 'Country'
          },
        ],

        newSong: {
          id: () => this.state.savedSongs.length + 1,
          title: '',
          createdOn: new Date(),
          lastEdited: 'someotherdate',
          isPublic: true,
          songLyrics: '',
          savedInstrumentals: [{ title: 'Get at Em' }],
          savedRecordings: [{ title: 'Flow 2' }],
          savedLyrics: [],
          uploadLyrics: [],
          tempRhymes: [],
          tempWords: [],
          genre: 'Country'
        },
      },
    }
  },

  methods: {
    clearLocalStorage() {
      localStorage.clear()
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
  @import url('https://fonts.googleapis.com/css?family=Comfortaa');

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
