<script lang="ts" setup>
import { useHead } from '@vueuse/head';
import { ref, onMounted } from 'vue'
import Header from './components/Header.vue'
import SearchVue from './components/Search.vue'
import HeroSection from './components/HeroSection.vue';
import Skeleton from './components/Skeleton.vue'
import SkeletonTheme from './components/SkeletonTheme.vue'

const data: any = ref({})

const loading = ref(true)

useHead({
  title: 'Etherscan Search',
  meta: [
    {
      name: 'description',
      content: 'Generated Vue3',
    },
    {
      name: 'viewport',
      content: 'width=device-width, initial-scale=1',
    },
  ],
  link: [
    {
      rel: 'icon',
      href: 'mini-logo.png',
    },
  ],
});

const fetchData = () => {
    // Title & Description
    data.value.title = 'Use Vue3/Skeleton!'
    data.value.description =
        'Lorem ipsum dolor sit amet consectetur, adipisicing elit. Odio, quisquam. Nostrum rerum sequi nulla consectetur, eos doloremque reprehenderit minus voluptate.'
    data.value.small = '@brayamvalero/vue3-skeleton'
    // User Profile
    data.value.img = 'https://i.imgur.com/Jvh1OQm.jpeg'
    data.value.name = 'Brayam Valero'
    data.value.role = 'Software Developer'
    data.value.email = 'example@gmail.com'
}

onMounted(() => {
    setTimeout(() => {
        fetchData()
    }, 3000)
})
</script>

<template>
    <div class="main">
        <Header></Header>
        <SearchVue></SearchVue>
        <!-- <HeroSection></HeroSection> -->
    </div>
</template>

<style>
.main {
  display: flex;
  flex-direction: column;
  align-items: center;
  min-height: 100vh;
  background-color: #161616;
  font-family: "Golos", sans-serif, Arial, Helvetica;
}
</style>
