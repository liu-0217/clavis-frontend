<template>
  <div class="py-10" data-cms="products-details-questions-answers">
    <h2
      class="uppercase text-3xl sm:text-4xl md:text-5xl font-medium mb-12 mx-10"
    >
      {{ $t('blocks.product.qna') }}
    </h2>

    <AccordionPanel
      v-for="(item, idx) in blockData"
      :key="item.id"
      :title="item.question"
      :description="item.answer"
      :class="{ 'mb-[32px]': idx + 1 !== blockData.length }"
    />
  </div>
</template>
<script setup lang="ts">
import type { PageBlock } from '../../../../../types/cms'
import AccordionPanel from '../../misc/AccordionPanel.vue'

defineProps<{
  blockData: PageBlock[]
}>()
</script>
