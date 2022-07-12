<template>
	<div class="app">
		<div class="main">
			<div v-if="!isLoading" class="actions">
				<div v-if="users.length < 1">
					<div class="param-form">
						<form>
							<input v-model="userItem.name" placeholder="Создать пользователя" />
						</form>
						<button @click="createUserItem">Сохранить</button>
					</div>
				</div>
				<div v-else>
					<div class="v-model-select">
						<select v-model="selectedUser">
							<option disabled value="">Выберите пользователя</option>
							<option v-for="user in users" :value="user.name" :key="user.name">
								{{ user.name }}
							</option>
						</select>

						<select v-show="selectedUser" v-model="selectedParameter">
							<option disabled value="">Выберите параметр</option>
							<option value="">Показать все параметры</option>
							<option
								v-for="param in loadedParams"
								:value="param.parameter_name"
								:key="param.id"
							>
								{{ param.parameter_name }}
							</option>
						</select>
					</div>
					<div class="param-form" v-show="selectedUser">
						<form>
							<input
								v-model="params.parameter_name"
								placeholder="Название параметра"
							/>
						</form>
						<form>
							<input v-model="params.parameter_type" placeholder="Тип параметра" />
						</form>
						<form>
							<input
								v-model="params.parameter_value"
								placeholder="Значение параметра"
							/>
						</form>
						<button @click="postParams">Добавить параметры</button>
					</div>
				</div>
			</div>
			<div v-else>Идет загрузка...</div>
			<UserItem
				:user="selectedUser"
				:selectedParameter="selectedParameter"
				@loadedParams="selectedParams"
			/>
		</div>
	</div>
</template>

<script>
import { axiosInstance } from "./api";
import UserItem from "./UserItem.vue";

export default {
	components: { UserItem },
	name: "usersParams",
	data() {
		return {
			userItem: {
				name: "",
			},
			users: [],
			selectedUser: "",
			selectedParameter: "",
			params: {
				parameter_name: "",
				parameter_type: "",
				parameter_value: "",
			},
			loadedParams: [],
			isLoading: false,
			isError: false,
		};
	},
	methods: {
		async createUserItem() {
			try {
				this.isLoading = true;
				const { data } = await axiosInstance.post("users/", this.userItem);
				this.users.push(data);
			} catch (error) {
				this.isError = true;
			} finally {
				this.isLoading = false;
			}
		},

		async fetchUsers() {
			try {
				this.isLoading = true;
				const { data } = await axiosInstance.get("users/");
				this.users = data.users;
			} catch (error) {
				this.isError = true;
			} finally {
				this.isLoading = false;
			}
		},
		async postParams() {
			try {
				this.isLoading = true;
				const queryString = `parameters/${this.selectedUser}/`;
				await axiosInstance.post(queryString, this.params);
			} catch (error) {
				this.isError = true;
			} finally {
				this.isLoading = false;
			}
		},
		selectedParams(data) {
			this.loadedParams = data;
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
	margin-top: 50px;
	display: flex;
	flex-direction: column;
	justify-content: center;
	align-items: center;
	height: 350px;
	width: 800px;
	background-color: rgb(135, 255, 177);
	border-radius: 15px;
}
.main {
	width: 700px;
	display: flex;
	justify-content: space-between;
}
.actions {
	display: flex;
	flex-direction: column;
	width: 350px;
}

select {
	width: 350px;
	outline: none;
	border: 2px solid rgb(255, 255, 255);
	border-radius: 15px;
	background-color: white;
	padding: 5px;
	box-sizing: border-box;
	margin-top: 10px;
	font-size: 16px;
	font-family: "Courier New", Courier, monospace;
}

input {
	height: 40px;
	width: 350px;
	border: 2px solid rgb(255, 255, 255);
	border-radius: 15px;
	outline: none;
	background-color: white;
	padding: 5px;
	box-sizing: border-box;
	margin-top: 10px;
	font-size: 16px;
	font-family: "Courier New", Courier, monospace;
}
input:focus {
	outline: none;
}
button {
	height: 40px;
	width: 350px;
	border: none;
	border-radius: 15px;
	outline: none;
	background-color: rgb(47, 227, 110);
	padding: 5px;
	box-sizing: border-box;
	margin-top: 10px;
	font-size: 16px;
	font-family: "Courier New", Courier, monospace;
}

button:hover {
	background-color: rgb(3, 255, 91);
}
</style>
