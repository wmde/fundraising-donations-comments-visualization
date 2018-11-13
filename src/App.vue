<template>
	<div id="app" class="container-fluid">
		<div class="title"><h1>{{ title }}</h1></div>
		<div class="row">
			<div class="theme-orange col-sm-3" align="center">
				<h4>Latest Comments</h4>
				<comment v-for="comment in comments.top"
								 :keyid="comment.id"
								 :amount="comment.betrag"
								 :name="comment.spender"
								 :content="comment.kommentar">
				</comment>
			</div>
			<div v-bind:class="{ 'theme-green': keyword === 'wikipedia', 'theme-blue': keyword === 'wissen', 'theme-purple': keyword === 'internet', 'col-sm-3': true }"
					 align="center"
					 v-for="keyword in keywords">
				<h4>#{{ keyword }}</h4>
				<comment v-for="comment in comments[ keyword ]"
								 :keyid="comment.id"
								 :amount="comment.betrag"
								 :name="comment.spender"
								 :content="comment.kommentar">
				</comment>
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
				keywords: [ 'wikipedia', 'wissen', 'internet' ],
				list: {},
				comments: {
					top: [],
					wikipedia: [],
					wissen: [],
					internet: []
				}
			}
		},
		methods: {
			fetchData: function () {
				this.$http.jsonp( 'https://spenden.wikimedia.de/list-comments.json?n=100&f=commentData', {
					jsonpCallback: 'commentData'
				} ).then( this.successCallback, this.errorCallback );
			},
			successCallback: function( data ) {
				let self = this;
				this.list = data.body;
				this.comments.top = this.filterTopComments( 4 );
				this.keywords.forEach( function ( keyword ) {
					self.comments[ keyword ] = self.filterByKeyword( keyword );
				} );
			},
			errorCallback: function() {
				console.log( 'error' );
			},
			filterByKeyword: function( keyword ) {
				return this.list.filter( comment => RegExp('\\b'+ keyword +'\\b').test( comment.kommentar.toLowerCase() ) ).slice( 0, 4 );
			},
			filterTopComments: function( limit ) {
				return this.list.slice( 0, limit );
			}
		},
		beforeMount: function() {
			this.fetchData();
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
