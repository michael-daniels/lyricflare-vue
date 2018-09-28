<template>
  <div class="song-tile">
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
  </div>
</template>

<script>
export default {
  name: 'SongTile',

  props: {
    song: Object
  },

  data() {
    return {

    }
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
  }

  .title {

  }

  .created-on {

  }

  .last-edited {

  }
</style>
