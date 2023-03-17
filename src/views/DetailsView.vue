<template>
	<Navbar />
	<Details :repoId="id" />
</template>

<script>
import Navbar from "@/components/navbar/Navbar.vue";
import Details from "@/components/details/Details.vue";
export default {
	components: {
		Navbar,
		Details,
	},
	props: ["id"],
};
</script>

<style></style>
