<template>
	<div id="app" class="container-fluid">
		<div class="title"><h3>{{ title }}</h3></div>
		<div class="row">
			<div class="theme-orange col-sm-4" align="center">
				<h4>Letzte Kommentare</h4>
				<comment v-for="comment in comments.top"
								 :keyid="comment.id"
								 :amount="comment.betrag"
								 :name="comment.spender"
								 :content="comment.kommentar">
				</comment>
			</div>
			<div v-bind:class="{ 'theme-green': keyword === 'wikipedia', 'theme-blue': keyword === 'wissen', 'col-sm-4': true }"
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
				keywords: [ 'wikipedia', 'wissen' ],
				list: {},
				comments: {
					top: [],
					wikipedia: [],
					wissen: []
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
				this.comments.top = this.filterTopComments( 6 );
				this.keywords.forEach( function ( keyword ) {
					self.comments[ keyword ] = self.filterByKeyword( keyword, 5 );
				} );
			},
			errorCallback: function() {
				console.log( 'error' );
			},
			filterByKeyword: function( keyword, limit ) {
				return this.list.filter( comment => RegExp('\\b'+ keyword +'\\b').test( comment.kommentar.toLowerCase() ) ).slice( 0, limit );
			},
			filterTopComments: function( limit ) {
				return this.list.filter( comment => RegExp(/^((?!wikipedia|wissen).)*$/).test( comment.kommentar.toLowerCase() ) ).slice( 0, limit );
			}
		},
		mounted: function() {
			this.fetchData();
			setInterval( function () {
				this.fetchData();
			}.bind( this ), 120000);
		}
	}
</script>

<style lang="scss">
	@import '../node_modules/bootstrap/scss/bootstrap.scss';
	body {
		font-family: system-ui, -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Lato, 'Oxygen-Sans', Ubuntu, Cantarell, 'Helvetica Neue', Helvetica, Arial, sans-serif;
		font-size: 18px;
		background: linear-gradient(90deg, white 23%, lightgray 23%);
	}
	#app {
		margin-top: 20px;
	}
	.title {
		width: 50%;
		margin: 0 auto;
	}
	h3 {
		font-weight: bold;
		border: 4px solid black;
		background: #f8f9fa;
		padding: 5px;
		text-align: center;
	}
	h4 {
		margin-top: 0.3em;
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
