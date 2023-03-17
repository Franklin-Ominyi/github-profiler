<template>
	<div class="home">
		<Navbar />
		<Home />
	</div>
</template>

<script>
// @ is an alias to /src
import Navbar from "@/components/navbar/Navbar.vue";
import Home from "@/components/home/Home.vue";

export default {
	name: "HomeView",
	components: {
		Navbar,
		Home,
	},
};
</script>

<style>
.home {
	position: relative;
}
</style>
