<template>
  <section class="max-w-2xl px-4 m-auto min-h-screen py-24 mt-4">
    <div class="hero bg-[#26264B] py-10 px-4 text-white text-center rounded-md">
      <img
        src="https://yazilimcigunleri.com/wp-content/uploads/2019/10/logo.png"
        alt="12 - 13 Ağustos Yazılımcı Günleri"
        srcset="https://yazilimcigunleri.com/wp-content/uploads/2019/10/logo.png 3x"
        class="mx-auto mb-8"
      >
      <h4 class="mb-2 text-2">
        {{ heroData.subtitle }}
      </h4>
      <h4 class="text-6xl font-semibold">
        {{ heroData.title }}
      </h4>
      <div class="flex items-center space-x-4 mx-auto w-max mt-8">
        <div v-for="item in heroData.list" :key="item.ID" class="flex items-center space-x-3">
          <img
            decoding="async"
            :src="item.icon.src"
            :alt="item.icon.alt"
            width="23"
            height="23"
          >
          <span>{{ item.label }}</span>
        </div>
      </div>
      <a :href="heroData.link.url" class="px-10 py-2 bg-purple-500 hover:bg-purple-400 inline-block text-white rounded-sm text-xl mt-8">
        {{ heroData.link.label }}
      </a>
    </div>
    <div v-if="logs && logs.length > 0" class="logs divide-y divide-gray-100">
      <LogCard v-for="log in logs" :key="log.ID" :log="log" />
    </div>
    <div v-else class="w-full text-gray-500 text-center mt-36">
      <h1 class="text-semibold text-xl">
        Ooopss! There are no logs to show.
      </h1>
    </div>
    <div v-if="showButton" class="w-full flex justify-center">
      <button
        class="text-blue-500 text-bold p-2 mt-6 flex justify-start items-center"
        @click="limit += 5"
      >
        Browse all updates <i class="ri-arrow-down-s-line text-2xl ml-2" />
      </button>
    </div>
  </section>
</template>

<script setup lang="ts">
import hero from '../content/contentrain/hero/hero.json'

const heroData = hero[0]

const limit = ref(5)
const { data } = useAsyncData(
  'logs',
  async () => await queryContent('logs').find()
)
const showButton = computed(() => {
  if (data.value?.length > limit.value) {
    return true
  } else {
    return false
  }
})

const logs = computed(() => {
  if (data.value?.length > limit.value) {
    return data.value.slice(
      0,
      limit.value < data.value.length ? limit.value : data.value.length
    )
  } else {
    return data.value
  }
})
</script>
