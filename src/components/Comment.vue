<template>
	<div id="comment">
		<div class="block">
			<div class="text">
				<p>{{ content }}</p>
				<p class="name">{{ name }}<span> {{ amount }} €</span></p>
			</div>
		</div>
	</div>
</template>

<script>
	export default {
		name: "comment",
		props: ['amount', 'name', 'content']
	}
</script>

<style scoped>
	.block {
		width: 80%;
		height: 100%;
		margin-top: 5%;
		padding: 10px;
		transition: box-shadow 0s;
		overflow: hidden;
		transform: scale(1.03);
		box-sizing: border-box;
		background-size: cover;
		box-shadow: 0 8px 6px -6px #979797;
		background-color: #f8f9fa;
	}
	.name {
		font-style: italic;
	}
	.name span {
		float: right;
	}
	.text{
		text-align: left;
	}
  p {
    margin-bottom: 0;
  }
</style>
