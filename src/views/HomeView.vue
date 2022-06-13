<template>
	<div class="home">
		<div v-if="projects.length">
			<div v-for="project in projects" :key="project.id">
				<SingleProject :project="project" @delete="handleDelete" @complete="handleComplete" />
			</div>
		</div>
		<div v-else>
			<p>No Project</p>
		</div>
	</div>
</template>

<script>
import SingleProject from "../components/SingleProject.vue";

export default {
	name: "HomeView",
	components: { SingleProject },
	data() {
		return {
			projects: [],
		};
	},
	methods: {
		handleDelete(id) {
			this.projects = this.projects.filter((project) => project.id !== id);
		},
		handleComplete(id) {
			let p = this.projects.find((project) => project.id === id);
			p.complete = !p.complete;
		},
	},
	mounted() {
		const fetchData = async () => {
			const response = await fetch("http://localhost:3000/projects");
			const data = await response.json();
			this.projects = data;
		};
		fetchData();
	},
};
</script>
