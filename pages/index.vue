<template>
  <main>
    <section class="hero bg-[#26264B] pt-48 pb-24 px-4 text-white rounded-md">
      <div class="max-w-2xl px-4 m-auto">
        <div class="">
          <img
            :src="heroData.logo.src"
            :alt="heroData.logo.alt"
            class="mx-auto mb-8 w-24 h-auto"
          >
        </div>
        <div class="w-full text-center">
          <h4 class="text-4xl font-semibold">
            {{ heroData.title }}
          </h4>
          <div class="flex items-center justify-center gap-4 mt-10 flex-wrap">
            <div v-for="item in heroData.list.slice(0,3) " :key="item.ID" class="flex items-center space-x-3">
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
        </div>
      </div>
      <div class="flex justify-center">
        <a :href="heroData.link.url" target="_blank" class="px-10 py-2 inline-block mx-auto bg-purple-500 hover:bg-purple-400 truncate max-w-xs text-white rounded-sm text-lg mt-12">
          {{ heroData.link.label }}
        </a>
      </div>
    </section>
    <section class="max-w-2xl px-4 m-auto min-h-screen py-24 mt-4">
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
  </main>
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
<style>
.line-clamp-1{
  overflow: hidden;
  display: -webkit-box;
  -webkit-box-orient: vertical;
  -webkit-line-clamp: 1;
}
.line-clamp-2{
  overflow: hidden;
  display: -webkit-box;
  -webkit-box-orient: vertical;
  -webkit-line-clamp: 2;
}
</style>
