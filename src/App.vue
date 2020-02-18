<template>
    <div id="app">
        <h1>Poor Man's Twitter</h1>
        <div class="content">
            <TweetForm   v-on:add-tweet="addTweet" />
            <TweetsTable v-bind:tweets="tweets"/>
        </div>
    </div>
</template>

<script>
  import TweetForm from './components/TweetForm.vue';
  import TweetsTable from './components/TweetsTable.vue';

  export default {
    name: 'App',
    components: {
      TweetForm,
      TweetsTable,
    },
    data: function () {
      return {
        tweets: []
      };
    },
    methods: {
      formatDate: function (d) {
        let date = d;
        if (!(date instanceof Date))
          date = new Date(date);

        let hours = date.getHours();
        let minutes = date.getMinutes();
        let ampm = hours >= 12 ? 'pm' : 'am';
        hours = hours % 12;
        hours = hours ? hours : 12; // the hour '0' should be '12'
        minutes = minutes < 10 ? '0' + minutes : minutes;
        let strTime = hours + ':' + minutes + ' ' + ampm;
        const day = `${date.getDate()}`.padStart(2, '0');
        const month = `${date.getMonth() + 1}`.padStart(2, '0');
        return `${strTime} ${day}-${month}-${date.getFullYear()}`;
      },
      loadTweets: function () {
        window.jQuery.get(
          'http://localhost:8000/',
          response => {
            if (response.status !== 'success') {
             console.log('An error has occurred!');
              return;
            }
            this.tweets = response.data;
          }
        );
      },
      addTweet: function (tweet) {
        this.tweets.push({
            name: tweet.name,
            message: tweet.message,
            datetime: tweet.datetime
        })
      }
    },

    mounted() {
      this.loadTweets();
    }
  }
</script>

<style>
    #app {
        font-family: Avenir, Helvetica, Arial, sans-serif;
        margin-top: 60px;
    }

    #app h1 {
        color: dodgerblue;
        -webkit-font-smoothing: antialiased;
        -moz-osx-font-smoothing: grayscale;
        text-align: center;
    }

    .content {
        display: flex;
        flex-direction: column;
        align-items: center;
        justify-content: center;
    }
</style>
