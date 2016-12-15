<template>
  <div id="app">

    <h1>palindrome</h1>

    <ul>
      <li v-for="text in content">
        {{text.msg}} -
        <span class="text-success" v-if="isPalindrome(text.msg)">is a palindrome</span>
        <span class="text-danger" v-else>is not a palindrome</span>
      </li>
    </ul>

    <h3>test a new one:</h3>

    <form class="form-inline" @submit.prevent="handleResult">
      <div class="form-group">
        <div class="input-group">
          <div class="input-group-addon"><span class="glyphicon glyphicon-sort" aria-hidden="true"></span></div>
          <input class="form-control" type="text" v-model="userText" @keyup.enter="handleResult" @blur="handleResult" placeholder="write here...">
        </div>
      </div>

      <button class="btn btn-default" @click="handleResult">check</button>

      <hr>

      <aside v-show="showResult">
        <!-- palindrome: {{palindrome}} -->
        <div class="alert alert-success" v-if="palindrome">Is a palindrome</div>
        <div class="alert alert-danger" v-else>Is not a palindrome</div>
      </aside>

    </form>

  </div>
</template>

<script>


const apiURL = '/static/content.json'

export default {
  data () {
    return {
      userText: '',
      palindrome: false,
      showResult: false,
      content: null
    }
  },

  created: function () {
    this.fetchData()
  },

  methods: {
    fetchData: function () {
      var xhr = new XMLHttpRequest()
      var self = this
      xhr.open('GET', apiURL)
      xhr.onload = function () {
        self.content = JSON.parse(xhr.responseText)
      }
      xhr.send()
    },

    isPalindrome: function(str) {
       str = str.toLowerCase().replace(/[^a-z]+/g,"");
       return str === str.split("").reverse().join("")
    },

    handleResult: function () {
      if(this.userText != '') {
        this.palindrome = this.isPalindrome(this.userText);
        this.showResult = true;
      } else {
        this.showResult = false;
      }
    }
  }
}
</script>

<style>
body {
  font-family: Helvetica, sans-serif;
}
</style>
