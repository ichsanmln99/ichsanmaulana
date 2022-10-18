<template>
	<div>
		<div class="prose mx-auto">
			<h1>{{ experiment.title }}</h1>
			<nuxt-content :document="experiment" />
		</div>
	</div>
</template>

<script>
export default {
	layout: 'layoutMainNavbar',
	async asyncData({ $content, params }) {
		const experiment = await $content('experiments', params.slug).fetch()

		return {
			experiment,
		}
	},
	head() {
		return {
			title: this.experiment.title,
			meta: [
				{
					hid: 'description',
					name: 'description',
					content: this.experiment.description,
				},
				// Open Graph
				{
					hid: 'og:title',
					property: 'og:title',
					content: this.experiment.title,
				},
				{
					hid: 'og:description',
					property: 'og:description',
					content: this.experiment.description,
				},
				// Twitter Card
				{
					hid: 'twitter:title',
					name: 'twitter:title',
					content: this.experiment.title,
				},
				{
					hid: 'twitter:description',
					name: 'twitter:description',
					content: this.experiment.description,
				},
			],
		}
	},
}
</script>

<style lang="css" scoped></style>
