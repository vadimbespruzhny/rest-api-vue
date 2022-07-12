<template>
	<div v-if="!isLoading">
		<div v-show="params.length > 0" class="userParams">
			<div v-if="!selectedParameter">
				<div v-for="param in params" :key="param.index">
					<div class="params">
						<div>{{ param.parameter_name }}:</div>
						<div>
							{{ param.parameter_value }}
							{{ param.parameter_type }}
						</div>
					</div>
				</div>
			</div>
			<div v-else>
				<div class="params">
					{{ parameter.parameter_value }}
					{{ parameter.parameter_type }}
				</div>
			</div>
		</div>
	</div>
	<div v-else>Идет загрузка...</div>
</template>

<script>
import { axiosInstance } from "./api";
export default {
	name: "userItem",
	data() {
		return {
			params: [],
			parameter: {},
			isLoading: false,
			isError: false,
		};
	},
	props: {
		user: {
			type: String,
		},
		selectedParameter: {
			type: String,
		},
	},
	methods: {
		async fetchUserParams() {
			if (this.user) {
				try {
					this.isLoading = true;
					const queryString = `parameters/${this.user}/`;
					const { data } = await axiosInstance.get(queryString);
					this.params = data.result;
					this.$emit("loadedParams", this.params);
				} catch (error) {
					this.isError = true;
				} finally {
					this.isLoading = false;
				}
			}
		},
		async fetchParameterItem() {
			if (this.selectedParameter) {
				try {
					this.isLoading = true;
					const queryString = `parameters/${this.user}/${this.selectedParameter}/`;
					const { data } = await axiosInstance.get(queryString);
					this.parameter = data.result;
				} catch (error) {
					this.isError = true;
				} finally {
					this.isLoading = false;
				}
			}
		},
	},
	computed: {
		userParams: function () {
			return this.fetchUserParams();
		},
		userParameter: function () {
			return this.fetchParameterItem();
		},
	},
};
</script>

<style scoped>
.userParams {
	display: flex;
	flex-direction: column;
	width: 260px;
	background-color: rgb(255, 255, 255);
	padding: 10px;
	box-sizing: border-box;
	border-radius: 15px;
	margin-top: 10px;
}
.params {
	display: flex;
	justify-content: space-between;
	font-size: 18px;
	font-family: "Courier New", Courier, monospace;
}
</style>
