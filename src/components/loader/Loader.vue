<template>
	<div class="app-loader">
		<div>
			<p>Loading</p>
			<i class="fa fa-spinner fa-spin fa-2x" aria-hidden="true"></i>
		</div>
	</div>
</template>

<script></script>

<style scoped>
.app-loader {
	height: 100%;
	width: 100%;
	z-index: 10 !important;
	display: flex;
	align-items: center;
	justify-content: center;
	gap: 10px;
	font-size: 20px;
}

.app-loader div {
	height: 62vh;
	display: flex;
	justify-content: center;
	align-items: center;
	gap: 10px;
	color: blue;
}
</style>
