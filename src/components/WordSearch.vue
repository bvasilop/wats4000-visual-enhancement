<template>
  <div>
    <div class="messages">
      <!-- TODO: Add message-container and supply messages property. -->
    </div>
    <div class="word-search">
      <form v-on:submit.prevent="findWords">
        <p><label>Find synonyms for <input type="text" v-model="phrase" placeholder="word or phrase"> that:</label></p>
        <ul>
          <li><label>sounds like <input type="text" v-model="soundsLike" placeholder="word or phrase"></label></li>
          <li><label>start with the letter <input type="text" v-model="startLetter" placeholder="single letter"></label></li>
          <li><label>end with the letter <input type="text" v-model="endLetter" placeholder="single letter"></label></li>
        </ul>
        <p><button type="submit">Search</button></p>
      </form>
    </div>
    <div class="word-list-container">
      <h2>Word List</h2>
      <ul class="word-list">
        <!-- TODO: Add transition-group around the list item here to animate items in the word list. -->
          <li v-for="word in wordList">{{ word }}&nbsp;<button v-on:click="removeWord(word)" class="remove-word">x</button></li>
      </ul>
    </div>
    <div class="results-container">
      <!-- TODO: Add spinner here to show when search is in progress. -->
      <spinner v-if="showSpinner"></spinner>
      <h2 v-if="results && results.length > 0">{{ results.length }} Words Found</h2>
      <ul v-if="results && results.length > 0" class="results">
        <!-- TODO: Add transition-group around the list item here to animate items in the results list. -->
          <li v-for="item in results" class="item">
            <p class="result-word">{{ item.word }}</p>
            <p><button v-on:click="addWord(item.word)" class="add-word">Add to WordList</button></p>
          </li>
      </ul>
      <!-- TODO: Add message to display here if no results are found. -->


    </div>
  </div>
</template>

<script>
import axios from 'axios';
// Note: vue2-animate is added using the require statement because it is a CSS file
require('vue2-animate/dist/vue2-animate.min.css');
// TODO: Import CubeSpinner for use as a child component
// TODO: Import MessageContainer for use as a child component


export default {
  name: 'WordSearch',
  components: {
    // TODO: Define child components here.

  },
  data () {
    return {
      results: null,
      wordList: [],
      messages: [],
      phrase: '',
      soundsLike: '',
      startLetter: '',
      endLetter: '',
      showSpinner: false
    }
  },
  methods: {
    addWord: function (word) {
      if (this.wordList.indexOf(word) === -1) {
        this.wordList.push(word);
        console.log(`Added ${word} to wordList.`);
        // TODO: Add message to this.messages to reflect this change.

      } else {
        console.log('Word is already on wordlist.');
        // TODO: Add message to this.messages to reflect this change.

      }
    },
    removeWord: function (word) {
      this.wordList.splice(this.wordList.indexOf(word), 1);
      // TODO: Add message to this.messages to reflect this change.

    },
    findWords: function() {
      // TODO: Show spinner when API request begins here.
      this.results = null;
      axios.get('https://api.datamuse.com/words', {
        params: {
          ml: this.phrase,
          sl: this.soundsLike,
          sp: `${this.startLetter}*${this.endLetter}`
        }
      })
      .then( response => {
        // TODO: Turn off spinner.
        this.results = response.data;
      })
      .catch( error => {
        // TODO: Turn off spinner

        // TODO: Add message to this.messages to display the errors.

      })
    }
  }
}
</script>

<style scoped>
.word-search {
  font-size: 1.2rem;
  white-space: nowrap;
  display: inline-block;
  width: 70%;
  float: left;
}
.word-list-container {
  display: inline-block;
  width: 25%;
  background: #e8e8e8;
  padding: 0.5rem;
}
.results-container {
  clear: both;
}
input[type="text"]{
  border-top: none;
  border-left: none;
  border-right: none;
  border-bottom: 1px solid #333;
  width: 300px;
  font-size: 1.2rem;
  color: #2c3e50;
  font-weight: 300;
  background: rgba(0,0,0,0.02);
  padding: 0.5rem;
}
button{
  background: #333;
  padding: 0.5rem;
  font-weight: 300;
  color: #fff;
  border: none;
  cursor: pointer;
  font-size: 1.4rem;
  border-radius: 0;
}
button.add-word {
  background: #e8e8e8;
  color: #333;
  font-size: 0.8rem;
}
button.add-word:hover {
  background: #fde300;
}
button.remove-word {
  font-size: 0.5rem;
  padding: 2px;
  display: inline-block;
  color: #333;
  background: none;
}
button.remove-word:hover {
  background: #aa0000;
  color: #fde300;
}
h1, h2 {
  font-weight: normal;
}
ul.results, ul.word-list {
  list-style-type: none;
  padding: 0;
}
.word-list li {
  margin: 5px 0;
  padding: 5px 0;
  border-bottom: 1px solid #333;
}
.results li {
  display: inline-block;
  margin: 10px;
  border: solid 1px #333;
  padding: 0.5rem;
  width: 200px;
  min-height: 100px;
  color: #fff;
  font-weight: 300;
  font-size: 1.2rem;
  background: rgba(0,0,0,0.7);
}
ul.errors {
  list-style-type: none;
}
.errors li {
  border: 1px solid red;
  color: red;
  padding: 0.5rem;
  margin: 10px 0;
}

a {
  color: #42b983;
}
</style>
