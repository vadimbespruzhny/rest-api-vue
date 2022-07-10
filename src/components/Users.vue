<template>
	<div class="app">
		<div class="users-list">
			<UserItem v-for="user in users" :user="user" :key="user.id"/>
		</div>
		<div style="background-color: lightblue; width: 500px">sdlkjflks</div>
	</div>
</template>

<script>
import axios from "axios";
import UserItem from "./UserItem.vue";
export default {
	components: { UserItem },
	name: "usersParams",
	data() {
		return {
			users: [],
		};
	},
	methods: {
		async fetchUsers() {
			try {
				const response = await axios.get("http://127.0.0.1:8000/api/users/");
				this.users = response.data.users;
			} catch (error) {
				console.log(error);
			}
		},
	},
	mounted() {
		this.fetchUsers();
	},
};
</script>

<style scoped>
.app {
	padding: 15px;
	display: flex;
	flex-direction: column;
	justify-content: center;
	align-items: center;
	height: 250px;
	background-color: bisque;
}
.users-list {
	background-color: aqua;
	height: 250px;
	width: 800px;
}
</style>
