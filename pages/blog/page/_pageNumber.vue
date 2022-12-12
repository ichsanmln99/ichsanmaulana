<template>
	<div>
		<div class="space-y-6">
			<template v-if="articles.length > 0">
				<card-article-title
					v-for="(article, index) in articles"
					:key="index"
					:title="article.title"
					:path="'/blog/' + article.slug"
					:createdAt="article.createdAt"
				/>
			</template>
			<template v-else-if="articelNotExists">
				<div class="text-center mb-16">
					<h1 class="text-3xl font-bold mb-3">Belum ada Artikel</h1>
					<NuxtLink class="link" to="/">ke Halaman Utama</NuxtLink>
				</div>
			</template>
			<template v-else>
				<div class="text-center mb-16">
					<h1 class="text-3xl font-bold mb-3">Artikel habis 😮</h1>
					<p class="opacity-80">
						Terima kasih sudah membaca blog saya. Tunggu updatean berikutnya, ya
					</p>
				</div>
			</template>

			<div class="flex justify-center" v-if="!articelNotExists">
				<NuxtLink
					:disabled="!previousPage"
					:to="previousPageUrl"
					class="btn btn-ghost"
					>Prev</NuxtLink
				>
				<NuxtLink :disabled="!nextPage" :to="nextPageUrl" class="btn btn-ghost"
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
	computed: {
		nextPageUrl() {
			return this.nextPage ? '/blog/page/' + (this.currentPageNumber + 1) : ''
		},
		previousPageUrl() {
			return this.previousPage
				? '/blog/page/' + (this.currentPageNumber - 1)
				: ''
		},
		articelNotExists() {
			return this.articles.length < 1 && this.currentPageNumber === 1
		},
	},
}
</script>
