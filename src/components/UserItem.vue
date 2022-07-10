<template>
	<div>{{ user.name }}</div>
	<div v-for="param in params" :key="param">
        {{ param.parameter_name }}
        {{ param.parameter_value }}
    </div>
	<button @click="fetchUserParams">Получить параметры</button>
</template>

<script>
import axios from "axios";
export default {
	name: "userItem",
	data() {
		return {
			params: [],
		};
	},
	props: {
		user: {
			type: Object,
			required: true,
		},
	},
	methods: {
		async fetchUserParams() {
			try {
				const instance = axios.create({
					baseURL: "http://127.0.0.1:8000/api/parameters/",
					timeout: 1000,
				});
				const { data } = await instance.get(this.user.name);
				this.params = data.result;
			} catch (error) {
				console.log(error);
			}
		},
	},
};
</script>

<style lang="scss" scoped></style>
