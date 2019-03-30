<template>
  <div class="container" id="app">
    <h4>#100DaysOfCode Stats</h4>
    <HelloWorld/>
    <footer>
      <div class="footer-copyright">
        <div>
        Created by 
        <a target="_blank" href="https://twitter.com/shahidcodes">@shahidcodes</a>
        </div>
      </div>
    </footer>
  </div>
</template>

<script>
import HelloWorld from "./components/HelloWorld";

export default {
  name: "App",
  components: {
    HelloWorld
  }
};
</script>

<style>
body {
  background-color: #eeeeee;
}
</style>
