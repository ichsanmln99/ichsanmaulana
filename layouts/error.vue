<template>
  <div class="text-center">
    <div class="mb-6">
      <h1 class="text-3xl font-bold" v-if="error.statusCode === 404">
        Yang kamu cari enggak ada 🍃
      </h1>
      <div v-else>
        <h1 class="text-3xl font-bold mb-3">duhh, ada error 🤦</h1>
        <p class="opacity-80">Coba lagi nanti, ya</p>
      </div>
    </div>
    <NuxtLink class="link" to="/">ke Halaman Utama</NuxtLink>
  </div>
</template>

<script>
export default {
  props: ['error'],
  layout: 'layoutMainNavbar', // you can set a custom layout for the error page
}
</script>
