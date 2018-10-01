<template>
  <div class="navbar">
    <div class="logo">
      <span class="logo-lyric"><i class="fas fa-fire"></i>lyric</span><span class="logo-flare">flare</span>
    </div>
    <div class="menu">
      <div class="menu-item-1">
        Hello, {{ this.state.currentUser.firstName }}
      </div>
      <div class="menu-item-2">
        <select class="select-menu" name="">
          <option value="">Profile</option>
          <option value="">Account</option>
          <option value="">Settings</option>
          <option v-if="state.loggedIn" value="">Logout</option>
          <option v-else value="">Login</option>
        </select>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'Navbar',

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

  .navbar {
    background-color: white;
    position: fixed;
    width: 90%;
    left: 0;
    box-shadow: 1px 1px 1px 1px silver;
    min-height: 5vw;
    display: flex;
    padding: 2vh 7vw;
    opacity: .9;
  }

  .logo {
    align-items: left;
    flex: 1;
    letter-spacing: 1px;
  }

  .logo-icon {
    max-width: 3rem;
  }

  .logo-lyric {
    color: limegreen;
    font-size: 40px;
    font-family: Baloo Paaji;
  }

  .logo-flare {
    color: gray;
    font-size: 40px;
    font-family: Baloo Paaji;
  }

  .fa-fire {
    font-size: 30px;
    margin-right: 3px;
  }

  .menu {
    flex: 1;
    display: flex;
    align-items: right;
  }

  .select-menu {
    width: 100%;
    border: none;
    height: 90%;
    font-size: 22px;
    text-align: center;
    font-family: K2D;
  }

  .menu-item-1 {
    flex: 1;
    text-align: center;
    padding: 30px 0;
    padding-left: 400px;
    font-family: K2D;
  }

  .menu-item-2 {
    padding: 20px 0;
  }

</style>
