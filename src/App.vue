<template>
  <div id="app">
    <b-navbar toggleable="lg" type="dark" variant="info">
        <b-navbar-toggle target="nav-text-collapse"></b-navbar-toggle>

        <b-navbar-brand>Weeber <i class="sub-title" >Robots</i></b-navbar-brand>

        <b-collapse id="nav-text-collapse" is-nav>
          <b-navbar-nav>
            <b-nav-text>By Sergio Rivera</b-nav-text>
          </b-navbar-nav>
        </b-collapse>

    
    </b-navbar>
    <router-view/>
  </div>
</template>

<style lang="less">
#app {
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}

#nav {
  padding: 30px;

  a {
    font-weight: bold;
    color: #2c3e50;

    &.router-link-exact-active {
      color: #42b983;
    }
  }
 
}
 .sub-title{
    font-weight: 200;
  }
</style>
