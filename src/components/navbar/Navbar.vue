<template>
	<nav>
		<div class="content">
			<router-link to="/"
				>Github <span style="color: blue">Profiler</span></router-link
			>
		</div>
	</nav>
</template>

<script>
export default {};
</script>

<style scoped>
nav {
	position: fixed;
	top: 0;
	width: 100%;
	padding: 20px 0;
	background-color: white;
	color: #ed1c24;
	border-bottom: 2px solid #ed1c24;
	z-index: 3;
}

.content {
	display: flex;
	align-items: center;
	gap: 10px;
}

a {
	font-size: 25px;
	font-weight: 600;
}

@media screen and (max-width: 760px) {
	nav {
		padding: 15px 0;
	}

	a {
		font-weight: 600;
	}
}
</style>
