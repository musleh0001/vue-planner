<template>
	<div class="home">
		<FilterNav @filterChange="current = $event" :current="current" />
		<div v-if="projects.length">
			<div v-for="project in filteredProjects" :key="project.id">
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
import FilterNav from "@/components/FilterNav.vue";

export default {
	name: "HomeView",
	components: { SingleProject, FilterNav },
	data() {
		return {
			projects: [],
			current: "all",
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
	computed: {
		filteredProjects() {
			if (this.current === "completed") {
				return this.projects.filter((project) => project.complete);
			}
			if (this.current === "ongoing") {
				return this.projects.filter((project) => !project.complete);
			}
			return this.projects;
		},
	},
};
</script>
