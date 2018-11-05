<template>
  <div id="app" class="container-fluid">
    <div class="title"><h1>{{ title }}</h1></div>
    <div class="row">
      <div class="theme-orange col-sm-3">
        <h4>Latest Comments</h4>
        <comment v-for="comment in comments"
                 :amount="comment.betrag"
                 :name="comment.spender"
                 :content="comment.kommentar">
        </comment>
      </div>
      <div class="hashtag-box col-sm-9">
        <div class="row">
          <div class="theme-green col-sm-4">
            <h4>#wikipedia</h4>
            <comment v-for="comment in comments"
                     :amount="comment.betrag"
                     :name="comment.spender"
                     :content="comment.kommentar">
            </comment>
          </div>
          <div class="theme-blue col-sm-4">
            <h4>#knowledge</h4>
            <comment v-for="comment in comments"
                     :amount="comment.betrag"
                     :name="comment.spender"
                     :content="comment.kommentar">
            </comment>
          </div>
          <div class="theme-purple col-sm-4">
            <h4>#love</h4>
            <comment v-for="comment in comments"
                     :amount="comment.betrag"
                     :name="comment.spender"
                     :content="comment.kommentar">
            </comment>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
  import Comment from './components/Comment.vue'

  export default {
    name: 'app',
    components: {
      Comment
    },
    data() {
      return {
        title: 'Herbstkampagne 2018 Spendenkommentare',
        comments: {}
      }
    },
    methods: {
      getCommentsList: function() {
        //this.$http.get( 'https://spenden.wikimedia.de/list-comments.json' ).then( this.successCallback, this.errorCallback );
        return {
          1: {
            "betrag": 15,
            "spender": "Jimmy Whales",
            "kommentar": "Gern unterst\u00fctze ich dieses gemeinn\u00fctzige Projekt...",
            "datum": "Thu, 01 Nov 2018 22:06:59 +0100",
            "id": 2827544
          },
          2: {
            "betrag": 50,
            "spender": "Anonymous",
            "kommentar": "Wikipedia ist mit Abstand das Beste, was das Internet zu bieten hat!",
            "datum": "Thu, 01 Nov 2018 14:12:58 +0100",
            "id": 2827277
          },
          3: {
            "betrag": 15,
            "spender": "Christian Schreiber",
            "kommentar": "Wissen f\u00fcr jedermann! Gut - kann aber noch besser werden. Ran an die Arbeit. Weiter so!",
            "datum": "Wed, 31 Oct 2018 10:22:25 +0100",
            "id": 2827544
          },
          4: {
            "betrag": 50,
            "spender": "Your biggest fan",
            "kommentar": "Habe schon öfter dieses Forum benutzt und immer sehr gute Informationen erhalten. Kann leider nicht mehr mehr spenden, da ich als Rentner Grundsicherung bekomme und davon bleibt so gut wie nichts übrig. Aber trotzdem vielen Dank für dieses großartige online-Lexikon",
            "datum": "Sat, 27 Oct 2018 15:35:50 +0200",
            "id": 2827277
          }
        }
      },
      successCallback: function( data ) {
        console.log( 'success' );
      },
      errorCallback: function() {
        console.log( 'error' );
      }
    },
    mounted: function() {
      this.comments = this.getCommentsList();
    }
  }
</script>

<style lang="scss">
  @import '../node_modules/bootstrap/scss/bootstrap.scss';
  body {
    font-family: system-ui, -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Lato, 'Oxygen-Sans', Ubuntu, Cantarell, 'Helvetica Neue', Helvetica, Arial, sans-serif;
    font-size: 16px;
    background: linear-gradient(90deg, white 18%, lightgray 18%);
  }
  #app {
    margin-top: 20px;
  }
  .title {
    width: 50%;
    margin: 0 auto;
  }
  h1 {
    font-weight: bold;
    margin-bottom: 0.5em;
    border: 4px solid black;
    background: #f8f9fa;
    padding: 5px;
    text-align: center;
  }
  h4 {
    margin-top: 1em;
  }
  .theme-orange {
    h4 {
      color: orange;
    }
    .block {
      border: 2px solid orange;
    }
  }
  .theme-green {
    h4 {
      color: #97b314;
    }
    .block {
      border: 2px solid #97b314;
    }
  }
  .theme-blue {
    h4 {
      color: #00adee;
    }
    .block {
      border: 2px solid #00adee;
    }
  }
  .theme-purple {
    h4 {
      color: purple;
    }
    .block {
      border: 2px solid purple;
    }
  }
</style>
