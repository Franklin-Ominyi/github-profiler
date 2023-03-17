<template>
	<Loader v-if="loading" />

	<main class="content" v-if="!loading && !error && data && repositories">
		<div class="details">
			<div class="img-container">
				<img :src="data.avatar_url" alt="Alegu Franklin" />
			</div>
			<div class="user-details">
				<div class="item">
					<p>Name:</p>
					<p>{{ data.name }}</p>
				</div>
				<div class="item">
					<p>Location:</p>
					<p>{{ data.location }}</p>
				</div>
				<div class="item">
					<p>Bio:</p>
					<p>{{ data.bio }}</p>
				</div>
				<div class="item">
					<p>Public Repos:</p>
					<p>{{ data.public_repos }}</p>
				</div>
				<div class="item">
					<p>Followers:</p>
					<p>{{ data.followers }}</p>
				</div>
				<div class="item">
					<p>Following:</p>
					<p>{{ data.following }}</p>
				</div>
				<div class="item">
					<p>Joined:</p>
					<p>{{ date.getDate() }}/{{ date.getMonth() }}/{{ date.getFullYear() }}</p>
				</div>
			</div>
		</div>

		<div class="list-repos">
			<div class="info">
				<h2>Repositories</h2>
				<p>Page {{ page }}</p>
			</div>

			<div
				class="list-item"
				v-for="item in repositories"
				:key="item.id"
				v-if="!loading && data && repositories"
			>
				<div>
					<p>
						<router-link :to="{ name: 'repo-details', params: { id: item.id } }">
							{{ item.name }}
						</router-link>
					</p>
					<p class="date">
						{{ new Date(item.created_at).getDate() }}/
						{{ new Date(item.created_at).getMonth() }}/
						{{ new Date(item.created_at).getFullYear() }}
					</p>
				</div>
			</div>

			<div class="btn-container">
				<div>
					<button class="disabled" v-if="page == 1">Prev</button>
					<button v-else @click="handlePrev">Prev</button>
				</div>
				<div>
					<button class="disabled" v-if="page >= Math.floor(data.public_repos / 10)">
						Next
					</button>

					<button v-else @click="handleNext">Next</button>
				</div>
			</div>
		</div>
	</main>
</template>

<script>
import { ref, watch, watchEffect } from "vue";
import axios from "axios";
import swal from "sweetalert";
import Loader from "../../../loader/Loader.vue";

export default {
	components: {
		Loader,
	},
	setup() {
		const input = ref("Franklin-Ominyi");
		const data = ref(null);
		const loading = ref(false);
		const error = ref(false);
		const page = ref(1);
		const repositories = ref([]);
		let date = ref();

		// Scroll back top
		watchEffect(() => {
			document.body.scrollTop = 0;
			document.documentElement.scrollTop = 0;
		});

		watch(page, async () => {
			let repos = await axios.get(
				`${data.value.repos_url}?per_page=10&page=${page.value}&sort=created`
			);
			repositories.value = repos.data;
			document.body.scrollTop = 0;
			document.documentElement.scrollTop = 0;
		});

		const handleSearch = async () => {
			if (!input.value || input.value.length <= 2) return;
			loading.value = true;
			error.value = false;
			repositories.value = [];
			try {
				let response = await axios.get(
					`https://api.github.com/users/${input.value}`
				);
				data.value = response.data;
				date.value = new Date(data.value.created_at);
				let repos = await axios.get(
					`${data.value.repos_url}?per_page=10&page=${page.value}&sort=created`
				);

				loading.value = false;
				repositories.value = repos.data;
			} catch (error) {
				console.log({ error });
				if (error.message == "Network Error") {
					swal("Oops!", "Check your internet connection then try again", "error");
				} else {
					swal("Oops!", "Something went wrong, please try again", "error");
				}

				loading.value = false;
			}
		};
		handleSearch();

		const handlePrev = () => (page.value = page.value - 1);
		const handleNext = () => (page.value = page.value + 1);

		return {
			input,
			loading,
			handleSearch,
			date,
			error,
			data,
			page,
			repositories,
			handleNext,
			handlePrev,
		};
	},
};
</script>

<style scoped>
.content {
	padding-bottom: 3rem;
}
/* Details */
.details {
	width: 100%;
	/* padding: 20px 0; */
}

.details .img-container {
	height: 400px;
	width: 400px;
}

.details .img-container img {
	border-radius: 50%;
	height: 100%;
	width: 100%;
}

.details .user-details {
	margin-top: 30px;
	display: flex;
	flex-direction: column;
	padding: 20px 0;
}

.details .user-details .item:first-child {
	border-top: 2px solid #ddd;
}

.details .user-details .item {
	display: flex;
	align-items: flex-start;
	gap: 20px;
	padding: 15px;
	border-bottom: 2px solid #ddd;
}

.details .user-details .item p:first-child {
	color: #ed1c24;
	font-weight: 500;
}

.details .user-details .item p:last-child {
	font-weight: 600;
}

/* Media Queries */

@media screen and (max-width: 760px) {
	.details .img-container {
		width: 90%;
		height: fit-content;
		margin: 0 auto;
	}
}

@media screen and (max-width: 760px) {
	h2 {
		font-size: 16px;
	}
}

/* Repository */
.list-repos {
	width: 100%;
	margin: 0 auto;
	position: relative;
	min-height: 60vh;
	margin-bottom: 7rem;
	margin-top: 2rem;
	height: auto;
}

.list-repos .info {
	display: flex;
	align-items: center;
	justify-content: space-between;
	margin-bottom: 10px;
}

.list-repos .info p {
	color: #ed1c24;
}

.list-repos h2 {
	color: blue;
	font-size: 25px;
}

.list-repos .list-item {
	padding: 10px;
	border-radius: 8px;
	background-color: #ddd;
	margin-bottom: 15px;
	display: flex;
	align-items: center;
	gap: 10px;
	transition: color 0.7s ease;
}

.list-repos .list-item p a {
	color: #ed1c24;
}

.list-repos .list-item p a:hover {
	color: #9d6062;
}

.list-repos .list-item p {
	color: white;
	font-weight: bold;
}

.list-repos .list-item .date {
	color: black;
	font-weight: 600;
	margin-top: 10px;
}

.list-repos .pagination-wrapper {
	margin-top: 25px;
}

.btn-container {
	display: flex;
	align-items: center;
	justify-content: space-between;
	margin-top: 20px;
}

.btn-container button {
	outline: none;
	border: 2px solid #ed1c24;
	border-radius: 8px;
	padding: 15px;
	cursor: pointer;
	transition: all 0.5s ease;
	color: #ed1c24;
}

.btn-container button:hover:not(.btn-container button.disabled) {
	background-color: #000;
}

.btn-container button.disabled {
	cursor: not-allowed;
	color: #ea838a;
	opacity: 0.6;
}

@media screen and (max-width: 760px) {
	.list-repos h2 {
		font-size: 16px;
	}
}
</style>
