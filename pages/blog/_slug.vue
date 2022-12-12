<template>
	<div>
		<div class="prose mx-auto">
			<h1>{{ article.title }}</h1>
			<nuxt-content :document="article" />
		</div>
	</div>
</template>

<script>
export default {
	layout: 'layoutMainNavbar',
	async asyncData({ $content, params }) {
		const article = await $content('articles', params.slug).fetch().catch(()=>([]))

		return {
			article,
		}
	},
	head() {
		return {
			title: this.article.title,
			meta: [
				{
					hid: 'description',
					name: 'description',
					content: this.article.description,
				},
				// Open Graph
				{ hid: 'og:title', property: 'og:title', content: this.article.title },
				{
					hid: 'og:description',
					property: 'og:description',
					content: this.article.description,
				},
				// Twitter Card
				{
					hid: 'twitter:title',
					name: 'twitter:title',
					content: this.article.title,
				},
				{
					hid: 'twitter:description',
					name: 'twitter:description',
					content: this.article.description,
				},
			],
		}
	},
}
</script>

<style lang="css" scoped></style>
