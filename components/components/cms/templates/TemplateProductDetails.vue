<template>
  <AppSignIn :is-open="isOpenSignIn" @close="isOpenSignIn = false" />
  <LoaderBlock v-if="pending || (isSlug && product?.length)" />
  <main v-else class="flex flex-col gap-2 mb-10">
    <n-breadcrumb class="mt-6 mb-10 px-10">
      <n-breadcrumb-item сlass="text-brand-gray">
        <NuxtLink to="/">{{ $t('common.main') }}</NuxtLink>
      </n-breadcrumb-item>
      <n-breadcrumb-item сlass="text-brand-gray">
        <NuxtLink :to="catalogPath">
          {{ $t(`mappers.${product?.product_type}`) }}
        </NuxtLink>
      </n-breadcrumb-item>
      <n-breadcrumb-item сlass="text-brand-gray">
        {{ product?.name }}
      </n-breadcrumb-item>
    </n-breadcrumb>

    <div class="flex flex-col gap-10">
      <ErrorBoundaryBlock>
        <HeaderBlock
          v-if="!pending"
          :block-data="product"
          :isOpenSignIn ="isOpenSignIn"
          :type="product?.product_type?.toLocaleLowerCase()"
          v-model="isOpenSignIn"
          
        >
          <AppButton
            v-show="product?.product_type !== 'Event'"
            @click="user.isLoggedIn?
              navigateTo(`/product/buy/${product.slug || route.params.id}`) : isOpenSignIn = true
            "
          >
            {{
              product?.product_type !== 'Acadeemy'
                ? $t('common.actions.buyAcademia')
                : $t('common.actions.buy')
            }}
          </AppButton>
        </HeaderBlock>
      </ErrorBoundaryBlock>

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
import { NBreadcrumb, NBreadcrumbItem } from 'naive-ui'
import { computed, ref } from 'vue'
import { useRoute } from 'vue-router'

import { checkIsEmpty } from '../../../utils/checkIsEmpty'
import { getApiAddress } from '../../../utils/getApiAddress'
import AppButton from '../../AppButton.vue'
import ErrorBoundaryBlock from '../blocks/misc/ErrorBoundaryBlock.vue'
import LoaderBlock from '../blocks/misc/LoaderBlock.vue'
import RichText from '../blocks/misc/RichText.vue'
import AboutCourse from '../blocks/products/details/AboutCourse.vue'
import AboutTutors from '../blocks/products/details/AboutTutors.vue'
import CourseProgram from '../blocks/products/details/CourseProgram.vue'
import HeaderBlock from '../blocks/products/details/HeaderBlock.vue'
import PaymentOptions from '../blocks/products/details/PaymentOptions.vue'
import QuestionsAnswers from '../blocks/products/details/QuestionsAnswers.vue'
import StudentWorks from '../blocks/products/details/StudentWorks.vue'
import { useUserStore } from '../../../store/user'


const user = useUserStore()
// Init template
const props = defineProps<{
  head?: { title: string; description: string }
}>()

const isOpenSignIn = ref(false)

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

const route = useRoute()

const { locale } = useI18n({ useScope: 'global' })

const isSlug = computed(() => !/^\d+$/.test(route.params.id as string))

const { data: product, pending } = await useAsyncData(
  'products',
  () =>
    $fetch(
      getApiAddress(
        isSlug.value
          ? `/api/v2/wagtail/products/`
          : `/api/v2/wagtail/products/${route.params.id}/`,
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

const catalogPath = computed(() => {
  switch (String(product.value?.product_type).toLocaleLowerCase()) {
    case 'course':
      return '/courses'
    case 'academy':
      return '/academies'
    default:
      return '/workshops'
  }
})

// Components for render
const blocksList = computed(() => [
  { name: AboutCourse, blockData: product.value?.body },
  { name: PaymentOptions, blockData: product.value?.purchase_options },
  { name: CourseProgram, blockData: product.value?.program },
  { name: AboutTutors, blockData: product.value?.instructors },
  { name: StudentWorks, blockData: product.value?.student_works },
  { name: QuestionsAnswers, blockData: product.value?.qna },
  { name: RichText, blockData: product.value?.seo_text },
])
</script>
