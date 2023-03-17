<template>
	<div>
		<MainComponent :repoId="repoId" />
		<FooterComponent />
	</div>
</template>

<script>
import MainComponent from "./components/main/Main.vue";
import FooterComponent from "../footer/Footer.vue";

export default {
	components: {
		MainComponent,
		FooterComponent,
	},
	props: ["repoId"],
};
</script>

<style scoped>
div {
	position: relative;
	padding-top: 130px;
	min-height: 100vh;
}
</style>
