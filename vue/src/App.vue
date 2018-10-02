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
        tempRhymes: [],
        savedSongs: [
          {
            id: 1,
            title: 'Make You Miss Me',
            createdOn: 'somedate',
            lastEdited: 'someotherdate',
            isPublic: false,
            showAds: true,
            savedInstrumentals: [{ title: 'Drop da Bass' }],
            savedRecordings: [{ title: 'Sample Melody' }],
            savedLyrics: null,
            tempNextWords: ['gun', 'left', 'hard'],
            getRhymes: this.getRhymes,
          },
          {
            id: 2,
            title: 'Killshot',
            createdOn: 'somedate',
            lastEdited: 'someotherdate',
            isPublic: true,
            savedInstrumentals: [{ title: 'Get at Em' }],
            savedRecordings: [{ title: 'Flow 2' }],
            savedLyrics: [],
            tempNextWords: [],
            getRhymes: this.getRhymes,
          },
          {
            id: 3,
            title: 'My Kind of Girl',
            createdOn: 'somedate',
            lastEdited: 'someotherdate',
            isPublic: true,
            savedInstrumentals: [{ title: 'Get at Em' }],
            savedRecordings: [{ title: 'Flow 2' }],
            savedLyrics: [],
            tempNextWords: [],
            getRhymes: this.getRhymes,
          },
          {
            id: 4,
            title: 'Over There',
            createdOn: 'somedate',
            lastEdited: 'someotherdate',
            isPublic: true,
            savedInstrumentals: [{ title: 'Get at Em' }],
            savedRecordings: [{ title: 'Flow 2' }],
            savedLyrics: [],
            tempNextWords: [],
            getRhymes: this.getRhymes,
          },
        ],
      },
    }
  },

  methods: {
    getRhymes(userInput) {
      console.log('tempRhymes', this.state.tempRhymes)
      fetch(`https://api.datamuse.com//words?rel_rhy=${userInput}`)
        .then((data) => {
          return data.json();
        })
        .then((rhymes) => {
          this.state.tempRhymes = rhymes;
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
  @import url('https://fonts.googleapis.com/css?family=Anonymous+Pro');

  * {
    margin: 0;
    padding: 0;
  }

  body {
    background-color: #f2f2f2;
    font-family: Anonymous Pro;
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
