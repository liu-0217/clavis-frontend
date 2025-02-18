<template>
  <LoaderBlock v-if="pending" />
  <main v-else class="flex flex-col gap-2 mb-10">
    <div class="flex flex-col">
      <ErrorBoundaryBlock v-for="(block, index) in blocksList" :key="index">
        <component
          :is="block.name"
          v-if="!checkIsEmpty(block.blockData)"
          :block-data="block.blockData"
        />
      </ErrorBoundaryBlock>
    </div>
  </main>
</template>
<script setup lang="ts">
import { computed } from 'vue'
import { useRoute } from 'vue-router'

import { getApiAddress } from '@/utils/getApiAddress'

import { checkIsEmpty } from '../../../utils/checkIsEmpty'
import Courses from '../blocks/instructor/Courses.vue'
import Header from '../blocks/instructor/Header.vue'
import StudentWorks from '../blocks/instructor/StudentWorks.vue'
import Video from '../blocks/instructor/Video.vue'
import ErrorBoundaryBlock from '../blocks/misc/ErrorBoundaryBlock.vue'
import LoaderBlock from '../blocks/misc/LoaderBlock.vue'

// Init template
const props = defineProps<{
  head?: { title: string; description: string }
}>()

const head = computed(() => props.head)
useHead({
  title: head.value?.title || 'Clavis',
  meta: [
    {
      name: 'description',
      content: head.value?.description || 'Clavis website page',
    },
  ],
  link: [
    {
      rel: 'canonical',
      href: 'https://clavis-schule.de/',
    },
  ],
})

// Init hooks
const { t } = useI18n({ useScope: 'global' })

// Store
const route = useRoute()

const { locale } = useI18n({ useScope: 'global' })

const isSlug = computed(() => !/^\d+$/.test(route.params.id as string))

const { data: instructor, pending } = await useAsyncData(
  'products',
  () =>
    $fetch(
      getApiAddress(
        isSlug.value
          ? `/api/v2/wagtail/instructors/`
          : `/api/v2/wagtail/instructors/${route.params.id}/`,
      ),
      {
        params: {
          locale: locale.value,
          fields: '*',
          slug: isSlug.value ? route.params.id : undefined,
        },
      },
    ).then(data => (isSlug.value ? data?.items[0] : data)),
  { watch: [locale, isSlug], deep: true },
)

const courses = computed(() => {
  const result = instructor?.value?.body?.find(
    block => block.type === 'teaches_in',
  )?.value

  if (!pending.value && result !== undefined) {
    return result
  }

  return {
    heading: t('blocks.tutorCourses'),
    cards: [],
  }
})

const studentWorks = computed(() => {
  const result = instructor?.value?.body?.find(
    block => block.type === 'art_block',
  )?.value

  if (!pending.value && result !== undefined) {
    return result
  }

  return undefined
})

const video = computed(() => {
  const result = instructor?.value?.body?.find(
    block => block.type === 'video_block',
  )?.value

  if (!pending.value && result !== undefined) {
    return result
  }

  return undefined
})

// Components for render
const blocksList = computed(() => [
  { name: Header, blockData: instructor.value },
  { name: Courses, blockData: courses.value },
  { name: StudentWorks, blockData: studentWorks.value },
  { name: Video, blockData: video.value },
])
</script>
