<template>
  <n-modal :show="isOpen">
    <div
      class="bg-white overflow-y-scroll max-w-[90%] md:max-w-[60%] h-fit rounded-md"
    >
      <div
        class="flex flex-col items-center gap-10 block-padding max-h-[600px] md:max-h-[650px]"
      >
        <h2 class="text-3xl text-brand-black">
          {{ $t('common.cookies.title') }}
        </h2>

        <div class="flex flex-col gap-4 text-brand-gray">
          <p>
            {{ $t('common.cookies.paragraph1') }}
          </p>
          <p>
            {{ $t('common.cookies.paragraph2') }}
          </p>
          <p>
            {{ $t('common.cookies.paragraph3') }}
          </p>
        </div>

        <div class="flex flex-col gap-2">
          <div class="bg-brand-light-gray flex flex-col gap-4 p-4 rounded-md">
            <div class="flex justify-between">
              <n-checkbox checked disabled class="whitespace-nowrap">
                {{ $t('common.cookies.options.essential.label') }}
              </n-checkbox>
              <a
                class="text-brand-black underline underline-offset- text-right"
                href="/docs/cookies.pdf"
                target="_blank"
                rel="noopener noreferrer"
              >
                {{ $t('common.cookies.options.essential.link') }}
              </a>
            </div>
            <div>
              {{ $t('common.cookies.options.essential.text') }}
            </div>
          </div>

          <div class="bg-brand-light-gray flex flex-col gap-4 p-4 rounded-md">
            <div class="flex justify-between">
              <n-checkbox
                v-model:checked="checkboxStatistic"
                class="whitespace-nowrap"
              >
                {{ $t('common.cookies.options.statistics.label') }}
              </n-checkbox>
              <a
                class="text-brand-black underline underline-offset- text-right"
                href="/docs/cookies.pdf"
                target="_blank"
                rel="noopener noreferrer"
              >
                {{ $t('common.cookies.options.statistics.link') }}
              </a>
            </div>
            <div>
              {{ $t('common.cookies.options.statistics.text') }}
            </div>
          </div>

          <div class="bg-brand-light-gray flex flex-col gap-4 p-4 rounded-md">
            <div class="flex justify-between">
              <n-checkbox
                v-model:checked="checkboxExternalMedia"
                class="whitespace-nowrap"
              >
                {{ $t('common.cookies.options.externalMedia.label') }}
              </n-checkbox>
              <a
                class="text-brand-black underline underline-offset- text-right"
                href="/docs/cookies.pdf"
                target="_blank"
                rel="noopener noreferrer"
              >
                {{ $t('common.cookies.options.externalMedia.link') }}
              </a>
            </div>
            <div>
              {{ $t('common.cookies.options.externalMedia.text') }}
            </div>
          </div>
        </div>

        <div class="flex flex-col md:flex-row w-full gap-4">
          <AppButton class="w-full md:w-1/2" @click="close">
            {{ $t('common.actions.acceptEssential') }}
          </AppButton>
          <AppButton class="w-full md:w-1/2" @click="closeAndAcceptAll">
            {{ $t('common.actions.acceptAll') }}
          </AppButton>
        </div>

        <div
          class="flex flex-col md:flex-row items-center text-center md:text-left gap-4 justify-center w-full pb-4 md:pb-10"
        >
          <NuxtLink
            class="w-fit inline-block text-brand-gray hover:text-brand-red"
            to="/legal/privacy"
          >
            <span>{{ $t('common.info.privacy') }}</span>
          </NuxtLink>
          <NuxtLink
            class="w-fit inline-block text-brand-gray hover:text-brand-red"
            to="/legal"
          >
            <span>{{ $t('common.info.legal') }}</span>
          </NuxtLink>
          <NuxtLink
            class="w-fit inline-block text-brand-gray hover:text-brand-red"
            to="/legal/terms"
          >
            <span>{{ $t('common.info.serviceTerms') }}</span>
          </NuxtLink>
        </div>
      </div>
    </div>
  </n-modal>
</template>
<script setup lang="ts">
import { NCheckbox, NModal } from 'naive-ui'
import { ref } from 'vue'

import AppButton from '@/components/AppButton.vue'

// State
const isOpen = ref(window.localStorage.getItem('is-closed-cookie') !== 'true')

const checkboxStatistic = ref(false)
const checkboxExternalMedia = ref(false)

// Actions
const close = () => {
  isOpen.value = false
  window.localStorage.setItem('is-closed-cookie', 'true')
}

const closeAndAcceptAll = () => {
  checkboxStatistic.value = true
  checkboxExternalMedia.value = true

  setTimeout(close, 100)
}
</script>
<style>
.n-modal-body-wrapper {
  max-width: 1440px !important;
  margin: auto !important;
}
</style>
