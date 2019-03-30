<template>
  <div>
    <div class="row">
      <div class="input-field col s12">
        <input v-model="username" id="username" type="text" class="validate">
        <label for="username">search by username</label>
      </div>
    </div>
    <div class="row">
      <div v-for="progress in userProgress" class="col s12 m6 l4" v-bind:key="progress.tweetId">
        <div class="card white darken-1">
          <div class="card-content black-text">
            <div class="row">
              <div class="col s4 m4">
                <img
                  :src="'https://avatars.io/twitter/'+progress.screen_name+'/medium'"
                  class="circle responsive-img"
                >
              </div>
              <div class="col s8 m8">
                <p>
                  <a :href="'https://twitter.com/'+progress.screen_name">@{{progress.screen_name}}</a>
                </p>
                <p>at #day{{parseInt(progress.currentDay)}}</p>
              </div>
            </div>
          </div>
          <div class="card-action">
            <a
              class="right waves-effect waves-grey"
              target="_blank"
              :href="'https://twitter.com/statuses/'+progress.tweetId"
            >Last tweet</a>
            <div class="clearfix"></div>
          </div>
        </div>
      </div>
      <div class="col s12 m12 l12">
        <div class="col s3">
          <a
            :disabled="this.currentPage == 0"
            v-on:click="prev"
            class="waves-effect waves-light btn red"
          >Previous</a>
        </div>
        <div class="col s3">
          <a v-on:click="next" class="waves-effect waves-light btn red">next</a>
        </div>
        <div class="col s3">
          <small>at page {{this.currentPage+1}}/{{this.pageRange}}</small>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import debounce from 'lodash.debounce';
export default {
  name: "HelloWorld",
  data() {
    return {
      userProgress: [],
      currentPage: 0,
      pageRange: 0,
      username: ""
    };
  },
  watch: {
    username: function(newVal, oldVal) {
      this.debouncedSearch()
    }
  },
  methods: {
    next: function(e) {
      // alert(1);
      if (this.currentPage < this.pageRange) {
        this.currentPage++;
        this.getData(this.currentPage);
      }
    },
    prev: function(e) {},
    getData: function(page) {
      fetch("https://capricious-oboe.glitch.me/user/progress/" + page)
        .then(response => response.json())
        .then(json => {
          this.userProgress = json.data;
          if (page == 0) {
            this.pageRange = Math.ceil(json.total / 10);
          }
        });
    },
    searchByUsername(){
      fetch("https://capricious-oboe.glitch.me/user/search/" + this.username)
        .then(response => response.json())
        .then(json => {
          this.userProgress = json.data;
          if (page == 0) {
            this.pageRange = Math.ceil(json.total / 10);
          }
        });
    }
  },
  mounted() {
    this.getData(this.currentPage);
    this.debouncedSearch = debounce(this.searchByUsername, 500);
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style>
</style>
