<!-- eslint-disable vuejs-accessibility/alt-text -->
<template>
  <div
    class="bg-brand-green block-padding-x mt-6 mb-12 relative text-white overflow-hidden"
  >
    <img
      src="/icons/aboutus/header_arcs.svg"
      alt="Arcs"
      class="absolute bottom-0 left-1/2 -translate-x-1/2 md:-translate-x-full translate-y-1/4"
    />

    <div class="relative mt-6 mb-4">
      <n-breadcrumb>
        <n-breadcrumb-item сlass="text-brand-gray">
          <NuxtLink to="/">{{ $t('common.main') }} / </NuxtLink>
        </n-breadcrumb-item>
        <n-breadcrumb-item сlass="text-brand-gray">
          {{ $t('common.aboutUs') }}
        </n-breadcrumb-item>
      </n-breadcrumb>
    </div>

    <!-- Header -->
    <h1
      class="relative text-[48px] md:text-[72px] leading-none uppercase text-white"
      v-html="$t('aboutUs.title')"
    />

    <div
      class="relative w-full mt-10 mb-14 md:mt-10 md:mb-14 md:pl-[50%] md:pr-[15%] text-xl text-white"
    >
      <p>
        {{ $t('aboutUs.subtitle') }}
      </p>
    </div>
  </div>

  <!-- About us -->
  <h2
    class="flex items-center justify-center text-center text-brand-red font-medium text-[28px] md:text-[36px] mt-8 md:mt-36 px-3 md:px-10"
  >
    {{ $t('aboutUs.target') }}
  </h2>

  <!-- Offers -->
  <div class="px-3 md:px-10 mt-6 md:mt-24 mb-10 text-brand-black">
    <h2
      class="font-semibold text-[36px] md:text-[56px] mb-[48px] text-brand-black uppercase"
    >
      {{ $t('aboutUs.offers.title') }}
    </h2>

    <div class="flex flex-col gap-6">
      <div class="grid grid-cols-1 md:grid-cols-2 gap-4">
        <div class="flex justify-center items-center">
          <img src="/icons/aboutus/person.svg" alt="Person" />
        </div>
        <div>
          <h3 class="text-brand-red text-[28px] sm:text-[36px] mb-6">
            {{ $t('aboutUs.offers.first.title') }}
          </h3>
          <h4 class="text-brand-gray font-medium mb-1">
            {{ $t('aboutUs.offers.first.subtitle1') }}
          </h4>
          <p class="text-brand-gray">
            {{ $t('aboutUs.offers.first.text1') }}
          </p>
          <h4 class="text-brand-gray font-medium mb-1">
            {{ $t('aboutUs.offers.first.subtitle2') }}
          </h4>
          <p class="text-brand-gray">
            {{ $t('aboutUs.offers.first.text2') }}
          </p>
        </div>
      </div>

      <div class="grid grid-cols-1 md:grid-cols-2 gap-4">
        <div class="flex justify-center items-center md:order-2">
          <img src="/icons/aboutus/masks.svg" alt="Masks" />
        </div>
        <div>
          <h3 class="text-brand-red text-[28px] sm:text-[36px] mb-6">
            {{ $t('aboutUs.offers.second.title') }}
          </h3>
          <h4 class="text-brand-gray font-medium mb-1">
            {{ $t('aboutUs.offers.second.subtitle1') }}
          </h4>
          <p class="text-brand-gray">
            {{ $t('aboutUs.offers.second.text1') }}
          </p>
          <h4 class="text-brand-gray font-medium mb-1">
            {{ $t('aboutUs.offers.second.subtitle2') }}
          </h4>
          <p class="text-brand-gray">
            {{ $t('aboutUs.offers.second.text2') }}
          </p>
        </div>
      </div>

      <div class="grid grid-cols-1 md:grid-cols-2 gap-4">
        <div class="flex justify-center items-center">
          <img src="/icons/aboutus/stars.svg" alt="Stars" />
        </div>
        <div>
          <h3 class="text-brand-red text-[28px] sm:text-[36px] mb-6">
            {{ $t('aboutUs.offers.third.title') }}
          </h3>
          <h4 class="text-brand-gray font-medium mb-1">
            {{ $t('aboutUs.offers.third.subtitle1') }}
          </h4>
          <p class="text-brand-gray">
            {{ $t('aboutUs.offers.third.text1') }}
          </p>
        </div>
      </div>

      <div class="grid grid-cols-1 md:grid-cols-2 gap-4">
        <div class="flex justify-center items-center md:order-2">
          <img src="/icons/aboutus/cubic.svg" alt="Cubic" />
        </div>
        <div>
          <h3 class="text-brand-red text-[28px] sm:text-[36px] mb-6">
            {{ $t('aboutUs.offers.fourth.title') }}
          </h3>
          <h4 class="text-brand-gray font-medium mb-1">
            {{ $t('aboutUs.offers.fourth.subtitle1') }}
          </h4>
          <p class="text-brand-gray">
            {{ $t('aboutUs.offers.fourth.text1') }}
          </p>
          <h4 class="text-brand-gray font-medium mb-1">
            {{ $t('aboutUs.offers.fourth.subtitle2') }}
          </h4>
          <p class="text-brand-gray">
            {{ $t('aboutUs.offers.fourth.text2') }}
          </p>
        </div>
      </div>
    </div>
  </div>

  <div class="bg-white text-brand-black">
    <LoaderBlock v-if="pending" />
    <PageConstructor
      v-else
      :blocks="[
        main.items[0].body.filter(item => item.type === 'running_text')?.[0],
      ]"
      class="flex flex-col"
    />
  </div>

  <LoadingBlock v-if="pendingInstructors" />
  <AboutTutors
    v-else
    :block-data="instructors.items.slice(0, 7)"
    :with-all-instructors="false"
    has-button
  />

  <!-- Principles -->
  <div
    class="grid grid-cols-1 gap-6 md:gap-0 md:grid-cols-2 mt-8 mb-10 mx-3 md:mx-10 text-brand-black"
  >
    <div>
      <h2
        class="font-semibold text-[36px] md:text-[56px] mb-[48px] uppercase text-brand-black"
      >
        {{ $t('aboutUs.principles.title') }}
      </h2>
      <img class="block ml-2" src="/icons/aboutus/dots.svg" />
    </div>

    <div class="grid grid-cols-1 md:grid-cols-2 gap-6 md:pr-2">
      <div class="rounded-[12px] bg-brand-light-gray p-4">
        <div class="flex items-center justify-center aspect-square">
          <img src="/icons/aboutus/heart.svg" alt="Heart" />
        </div>

        <AppDivider class="my-4 px-4" />

        <p class="font-medium text-brand-black text-xl">
          {{ $t('aboutUs.principles.item1') }}
        </p>
      </div>
      <div class="rounded-[12px] bg-brand-light-gray p-4">
        <div class="flex items-center justify-center aspect-square">
          <img src="/icons/aboutus/key.svg" alt="Key" />
        </div>

        <AppDivider class="my-4 px-4" />

        <p class="font-medium text-brand-black text-xl">
          {{ $t('aboutUs.principles.item2') }}
        </p>
      </div>
    </div>

    <div class="grid grid-cols-1 md:grid-cols-2 gap-6 text-brand-black">
      <div class="rounded-[12px] bg-brand-light-gray p-4">
        <div class="flex items-center justify-center aspect-square">
          <img src="/icons/aboutus/books.svg" alt="Books" />
        </div>

        <AppDivider class="my-4 px-4" />

        <p class="font-medium text-brand-black text-xl">
          {{ $t('aboutUs.principles.item3') }}
        </p>
      </div>
      <div class="rounded-[12px] bg-brand-light-gray p-4">
        <div class="flex items-center justify-center aspect-square">
          <img src="/icons/aboutus/star.svg" alt="Star" />
        </div>

        <AppDivider class="my-4 px-4" />

        <p class="font-medium text-brand-black text-xl">
          {{ $t('aboutUs.principles.item4') }}
        </p>
      </div>
    </div>

    <div class="relative py-32 md:py-0">
      <img
        class="absolute top-1/2 left-1/2 w-full md:w-4/5 -translate-x-1/2 -translate-y-1/2 z-0"
        alt="Border"
        src="/icons/aboutus/border.svg"
      />
      <div
        class="md:absolute top-1/2 left-1/2 w-full md:-translate-x-1/2 md:-translate-y-1/2 z-10 flex flex-col items-center justify-center gap-3 md:gap-6"
      >
        <img
          src="/icons/aboutus/quotes.svg"
          alt="Quotes"
          class="scale-[1.2] lg:scale-100"
        />
        <p class="font-semibold text-[16px] md:text-[24px] w-3/5">
          {{ $t('aboutUs.principles.quote.text') }}
        </p>
        <p class="text-brand-gray">
          {{ $t('aboutUs.principles.quote.author') }}
        </p>
      </div>
    </div>
  </div>

  <RunningMedia
    :block-data="
      instructors?.items
        .slice(0, 8)
        .map(instructor =>
          getApiAddress(instructor.title_image.meta.download_url),
        ) || []
    "
  />

  <!-- Approach -->
  <div
    class="text-brand-black flex flex-col items-center gap-6 lg:gap-0 lg:flex-row aspect-square max-h-[1000px] max-w-[1000px] min-w-base mx-3 lg:mx-auto relative mt-3 lg:mt-10"
  >
    <h2
      class="uppercase text-brand-black lg:order-10 lg:text-[56px] lg:absolute lg:-translate-y-1/2 lg:-translate-x-1/2 font-semibold text-[30px] top-1/2 lg:left-1/2 text-center"
    >
      {{ $t('aboutUs.approach.title') }}
    </h2>

    <div
      class="hidden lg:block border-[1px] border-brand-red rounded-[1000px] top-1/2 left-1/2 -translate-x-1/2 -translate-y-1/2 h-full w-full border-dashed absolute z-0"
    />
    <div
      class="lg:-translate-y-1/4 lg:absolute z-10 top-1/4 rounded-[12px] bg-brand-light-gray p-6 max-w-[500px]"
    >
      <p class="flex items-center">
        <img src="/icons/star.svg" alt="Star" />
        <span class="text-[20px] lg:text-[24px] ml-1 text-brand-red">
          {{ $t('aboutUs.approach.item1.text') }}
        </span>
      </p>
      <p class="mt-4 text-[18px]">
        {{ $t('aboutUs.approach.item1.title') }}
      </p>
    </div>

    <div
      class="text-brand-black lg:absolute lg:-translate-y-1/4 z-10 top-1/4 right-0 rounded-[12px] bg-brand-light-gray p-6 max-w-[500px]"
    >
      <p class="flex items-center">
        <img src="/icons/star.svg" alt="Star" />
        <span class="text-[20px] lg:text-[24px] ml-1 text-brand-red">
          {{ $t('aboutUs.approach.item2.text') }}
        </span>
      </p>
      <p class="mt-4 text-[18px]">
        {{ $t('aboutUs.approach.item2.title') }}
      </p>
    </div>

    <div
      class="lg:translate-y-1/4 lg:absolute z-10 bottom-1/4 rounded-[12px] bg-brand-light-gray p-6 max-w-[500px] text-brand-black"
    >
      <p class="flex items-center">
        <img src="/icons/star.svg" alt="Star" />
        <span class="text-[20px] lg:text-[24px] ml-1 text-brand-red">
          {{ $t('aboutUs.approach.item3.text') }}
        </span>
      </p>
      <p class="mt-4 text-[18px]">
        {{ $t('aboutUs.approach.item3.title') }}
      </p>
    </div>

    <div
      class="lg:translate-y-1/4 lg:absolute z-10 bottom-1/4 right-0 rounded-[12px] bg-brand-light-gray p-6 max-w-[500px] text-brand-black"
    >
      <p class="flex items-center">
        <img src="/icons/star.svg" alt="Star" />
        <span class="text-[20px] lg:text-[24px] ml-1 text-brand-red">
          {{ $t('aboutUs.approach.item3.text') }}
        </span>
      </p>
      <p class="mt-4 text-[18px]">
        {{ $t('aboutUs.approach.item3.title') }}
      </p>
    </div>

    <div
      class="lg:-translate-x-1/2 lg:absolute lg:translate-y-1/2 z-10 bottom-0 left-1/2 rounded-[12px] bg-brand-light-gray p-6 max-w-[500px] mt-8 text-brand-black"
    >
      <p class="flex items-center">
        <img src="/icons/star.svg" alt="Star" />
        <span class="text-[20px] lg:text-[24px] ml-1 text-brand-red">
          {{ $t('aboutUs.approach.item4.text') }}
        </span>
      </p>
      <p class="mt-4 text-[18px]">
        {{ $t('aboutUs.approach.item4.title') }}
      </p>
    </div>
  </div>

  <!-- Values -->
  <div class="mt-[120px] bg-brand-light-gray pt-12 pb-8 text-brand-black">
    <h2
      class="font-semibold text-[36px] md:text-[56px] mb-[48px] px-3 md:px-10 text-brand-black uppercase"
    >
      {{ $t('aboutUs.values.title') }}
    </h2>

    <AppDivider class="mb-12" />

    <div class="grid grid-cols-1 md:grid-cols-3 px-3 md:px-10 gap-6">
      <h3 class="text-brand-red text-[28px] md:text-[36px]">
        {{ $t('aboutUs.values.item1.title') }}
      </h3>
      <div class="col-span-2 mr-8">
        <h4 class="text-brand-gray font-medium mb-1">
          {{ $t('aboutUs.values.item1.subtitle1') }}
        </h4>
        <p class="text-brand-gray mb-1">
          {{ $t('aboutUs.values.item1.text1') }}
        </p>
        <h4 class="text-brand-gray font-medium mb-1">
          {{ $t('aboutUs.values.item1.subtitle2') }}
        </h4>
        <p class="text-brand-gray mb-1">
          {{ $t('aboutUs.values.item1.text2') }}
        </p>
      </div>
    </div>

    <AppDivider class="mb-12 mt-12" />

    <div class="grid grid-cols-1 md:grid-cols-3 px-3 md:px-10 gap-6">
      <h3 class="text-brand-red text-[28px] md:text-[36px]">
        {{ $t('aboutUs.values.item2.title') }}
      </h3>
      <div class="col-span-2 mr-8">
        <h4 class="text-brand-gray font-medium mb-1">
          {{ $t('aboutUs.values.item2.subtitle1') }}
        </h4>
        <p class="text-brand-gray mb-1">
          {{ $t('aboutUs.values.item2.text1') }}
        </p>
        <h4 class="text-brand-gray font-medium mb-1">
          {{ $t('aboutUs.values.item2.subtitle2') }}
        </h4>
        <p class="text-brand-gray mb-1">
          {{ $t('aboutUs.values.item2.text2') }}
        </p>
      </div>
    </div>

    <AppDivider class="mb-12 mt-12" />

    <div class="grid grid-cols-1 md:grid-cols-3 px-3 md:px-10 gap-6">
      <h3 class="text-brand-red text-[28px] md:text-[36px]">
        {{ $t('aboutUs.values.item3.title') }}
      </h3>
      <div class="col-span-2 mr-8">
        <h4 class="text-brand-gray font-medium mb-1">
          {{ $t('aboutUs.values.item3.subtitle1') }}
        </h4>
        <p class="text-brand-gray mb-1">
          {{ $t('aboutUs.values.item3.text1') }}
        </p>
        <h4 class="text-brand-gray font-medium mb-1">
          {{ $t('aboutUs.values.item3.subtitle2') }}
        </h4>
        <p class="text-brand-gray mb-1">
          {{ $t('aboutUs.values.item3.text2') }}
        </p>
      </div>
    </div>

    <AppDivider class="mb-12 mt-12" />

    <div class="grid grid-cols-1 md:grid-cols-3 px-3 md:px-10 gap-6 pb-12">
      <h3 class="text-brand-red text-[28px] md:text-[36px]">
        {{ $t('aboutUs.values.item4.title') }}
      </h3>
      <div class="col-span-2 mr-8">
        <h4 class="text-brand-gray font-medium mb-1">
          {{ $t('aboutUs.values.item4.subtitle1') }}
        </h4>
        <p class="text-brand-gray mb-1">
          {{ $t('aboutUs.values.item4.text1') }}
        </p>
      </div>
    </div>
  </div>

  <div class="bg-white pb-8">
    <LoaderBlock v-if="pending" />
    <PageConstructor
      v-else
      :blocks="main.items[0].body.filter(item => item.type === 'social_media')"
      class="flex flex-col"
    />
  </div>
</template>
<script setup lang="ts">
import RunningMedia from '@/components/cms/blocks/main/RunningMedia.vue'
import { getApiAddress } from '@/utils/getApiAddress'

import AppDivider from '../components/AppDivider.vue'
import LoaderBlock from '../components/cms/blocks/misc/LoaderBlock.vue'
import AboutTutors from '../components/cms/blocks/products/details/AboutTutors.vue'
import PageConstructor from '../components/cms/PageConstructor.vue'

const { locale } = useI18n({ useScope: 'global' })

const { data: main, pending } = useAsyncData(
  'homepage',
  () =>
    $fetch(getApiAddress(`/api/v2/wagtail/homepage/`), {
      params: {
        locale: locale.value,
      },
    }),
  { watch: [locale] },
)

const { data: instructors, pending: pendingInstructors } = await useAsyncData(
  'instructors',
  () =>
    $fetch(getApiAddress(`/api/v2/wagtail/instructors/`), {
      params: {
        locale: locale.value,
        fields: '*',
      },
    }),
  { watch: [locale], deep: true },
)
</script>
<style scoped>
.min-w-base {
  min-width: min(100%, 1200px);

  @media screen and (max-width: 1024px) {
    min-width: auto;
  }
}
</style>
