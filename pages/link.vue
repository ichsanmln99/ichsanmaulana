<template>
	<div>
		<div class="prose mx-auto">
			<h1>{{ link.title }}</h1>
			<nuxt-content :document="link" />
		</div>
	</div>
</template>

<script>
export default {
	layout: 'layoutMainNavbar',
	async asyncData({ $content, params }) {
		const link = await $content('links', params.slug).fetch()

		return {
			link,
		}
	},
	head() {
		return {
			title: this.link.title,
			meta: [
				{
					hid: 'description',
					name: 'description',
					content: this.link.description,
				},
				// Open Graph
				{ hid: 'og:title', property: 'og:title', content: this.link.title },
				{
					hid: 'og:description',
					property: 'og:description',
					content: this.link.description,
				},
				// Twitter Card
				{
					hid: 'twitter:title',
					name: 'twitter:title',
					content: this.link.title,
				},
				{
					hid: 'twitter:description',
					name: 'twitter:description',
					content: this.link.description,
				},
			],
		}
	},
}
</script>

<style lang="css" scoped></style>
