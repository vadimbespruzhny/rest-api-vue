<template>
	<Users/>
</template>

<script>
import Users from "./components/Users.vue";

export default {
	name: "App",
	components: {
		Users,
	},
};
</script>

<style>
body {
	background-color: rgb(245, 235, 255);
}
#app {
	margin: 0 auto;
	display: flex;
	justify-content: center;
}
</style>
