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
            title: 'Make You Miss Me',
            createdOn: 'somedate',
            lastEdited: 'someotherdate',
            isPublic: false,
            savedInstrumentals: [{ title: 'Drop da Bass' }],
            savedRecordings: [{ title: 'Sample Melody' }],
            savedLyrics: [],
            tempNextWords: [],
            tempRhymes: [],
          },
          {
            title: 'Killshot',
            createdOn: 'somedate',
            lastEdited: 'someotherdate',
            isPublic: true,
            savedInstrumentals: [{ title: 'Get at Em' }],
            savedRecordings: [{ title: 'Flow 2' }],
            savedLyrics: [],
            tempNextWords: [],
            tempRhymes: [],
          },
          {
            title: 'My Kind of Girl',
            createdOn: 'somedate',
            lastEdited: 'someotherdate',
            isPublic: true,
            savedInstrumentals: [{ title: 'Get at Em' }],
            savedRecordings: [{ title: 'Flow 2' }],
            savedLyrics: [],
            tempNextWords: [],
            tempRhymes: [],
          },
          {
            title: 'Over There',
            createdOn: 'somedate',
            lastEdited: 'someotherdate',
            isPublic: true,
            savedInstrumentals: [{ title: 'Get at Em' }],
            savedRecordings: [{ title: 'Flow 2' }],
            savedLyrics: [],
            tempNextWords: [],
            tempRhymes: [],
          },
        ],
      },
    }
  },
}
</script>

<style>
  @import url('https://fonts.googleapis.com/css?family=Indie+Flower');
  @import url('https://fonts.googleapis.com/css?family=K2D');

  * {
    margin: 0;
    padding: 0;
  }

  body {
    background-color: #f2f2f2;
  }

  h1 {
    color: gray;
    font-family: K2D;
  }

  h2 {
    color: limegreen;
    font-family: K2D;
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
