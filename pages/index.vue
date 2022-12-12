<template>
  <div>
    <div class="mb-12">
      <img
        src="/profile.jpg"
        class="mask mask-circle mb-8 lg:w-96 w-80 lg:w-60 w-48 mx-auto"
      />
      <div class="text-center">
        <h1 class="font-extrabold text-3xl">Bojjidev</h1>
        <p class="opacity-80 mb-4">a Web Developer Notes</p>
        <div class="space-x-2">
          <NuxtLink class="btn btn-sm btn-ghost" to="/about"
          >👨 Tentang Saya</NuxtLink
        >
          <NuxtLink class="btn btn-sm btn-ghost" to="/contact"
            >📬 Kontak</NuxtLink
          >
        </div>
      </div>
    </div>
    <div class="mb-10">
      <div class="mb-8">
        <h2 class="font-extrabold text-xl mb-2">Hai 👋</h2>
        <p class="opacity-80">
          Salam Kenal, nama saya Bojji (bukan nama asli), Seorang Web Developer. 
          Halaman ini saya buat sebagai catatan belajar dan media berbagi tentang apa yang saya suka.
        </p>
      </div>
      <ul class="menu-list space-y-4">
        <li>
          <NuxtLink class="link" to="/blog/page/1">📔 Tulisan Blog</NuxtLink>
        </li>
        <li>
          <NuxtLink class="link" to="/link">🖇 Link Bermanfaat</NuxtLink>
        </li>
        <li>
          <NuxtLink class="link" to="/experiment"
            >🏗 Karya / Eksperimen</NuxtLink
          >
        </li>
      </ul>
    </div>

    <div v-if="articles.length > 0">
      <h3 class="font-extrabold text-lg mb-4">Tulisan Baru</h3>
      <div class="space-y-6">
        <card-article-title
          v-for="(article, index) in articles"
          :key="index"
          :title="article.title"
          :path="'/blog/' + article.slug"
          :created-at="article.createdAt"
        />
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'IndexPage',
  layout: 'layoutDefault',
  async asyncData({ $content }) {
    const articles = await $content('articles')
      .limit(3)
      .sortBy('createdAt', 'desc')
      .fetch()
      .catch(()=>([]))

    return {
      articles,
    }
  },

  methods: {
    methodName() {},
  },
}
</script>
