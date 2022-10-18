<template>
	<div>
		<div class="space-y-6">
			<card-article-title
				v-for="(article, index) in articles"
				:key="index"
				:title="article.title"
				:path="'/blog/' + article.slug"
				:createdAt="article.createdAt"
			/>

			<div class="flex justify-center">
				<NuxtLink
					:disabled="!previousPage"
					:to="'/blog/page/' + (currentPageNumber - 1)"
					class="btn btn-ghost"
					>Prev</NuxtLink
				>
				<NuxtLink
					:to="'/blog/page/' + (currentPageNumber + 1)"
					class="btn btn-ghost"
					>Next</NuxtLink
				>
			</div>
		</div>
	</div>
</template>

<script>
export default {
	layout: 'layoutMainNavbar',
	async asyncData({ $content, params, error }) {
		const currentPageNumber = parseInt(params.pageNumber)
		const numberArticles = 10

		const articles = await $content('articles')
			.only(['title', 'createdAt', 'slug'])
			.limit(numberArticles)
			.skip(numberArticles * (currentPageNumber - 1))
			.sortBy('createdAt', 'desc')
			.fetch()

		if (!articles.length) {
			return error({ statusCode: 404, message: 'No articles found!' })
		}

		const nextPage = articles.length === numberArticles
		const previousPage = currentPageNumber > 1

		return {
			articles,
			nextPage,
			previousPage,
			currentPageNumber,
			params,
		}
	},
}
</script>
