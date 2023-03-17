<template>
	<footer>
		<div class="content">
			&copy;{{ date }} All Rights Reserved | Developed by
			<a class="name" href="https://frankofficial.netlify.com">Alegu Franklin</a>
		</div>
	</footer>
</template>

<script>
import { ref } from "vue";

export default {
	setup() {
		const date = ref();

		date.value = new Date().getFullYear();

		return {
			date,
		};
	},
};
</script>

<style scoped>
footer {
	position: absolute;
	bottom: 0;
	width: 100%;
	background-color: black;
	color: white;
	padding: 10px 0;
	text-align: center;
	font-size: 16px;
}

.name {
	color: #ed1c24;
	font-weight: 500;
	transition: color 0.5s ease;
}
.name:hover {
	color: #ea9396;
}
</style>
