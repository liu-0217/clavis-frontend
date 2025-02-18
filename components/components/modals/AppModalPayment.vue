<template>
  <n-modal :show="isOpen" @mask-click="$emit('close')">
    <div
      class="fixed w-[90%] md:w-[70%] h-fit bg-white rounded-md top-1/2 left-1/2 -translate-x-2/4 -translate-y-2/4 overflow-y-auto"
    >
      <div class="flex flex-col">
        <div
          class="flex justify-between items-center border-b-[1px] border-brand-black py-[16px]"
        >
          <h1 class="font-medium text-4xl ml-10">
            {{ $t('common.modals.editSubscription') }}
          </h1>

          <div
            class="flex gap-[20px] items-center mr-10"
            @click="$emit('close')"
          >
            <button
              class="bg-white border-[1px] border-brand-black w-[35px] h-[35px] rounded-full flex items-center justify-center hover:bg-brand-light-gray transition ease-in delay-100 transform active:scale-[0.93]"
            >
              <img
                src="/icons/cross.svg"
                alt="close"
                class="w-[15px] h-[15px]"
              />
            </button>
          </div>
        </div>

        <div class="flex flex-col gap-4 m-10">
          <template v-if="order.paypal_invoice_link">
            <div class="w-full py-2 flex justify-between items-center">
              <p>{{ $t('common.modals.subscriptionPaymentPayPal') }}</p>
              <a
                :href="
                  !!order.paypal_invoice_link &&
                  ['awaiting payment', 'renewal available'].includes(
                    order.state,
                  )
                    ? order.paypal_invoice_link
                    : '#'
                "
                target="_blank"
                rel="noopener noreferrer"
                ><AppButton
                  :disabled="
                    !order.paypal_invoice_link ||
                    ['awaiting payment', 'renewal available']!.includes(
                      order.state,
                    )
                  "
                  >{{ $t('common.actions.pay') }}</AppButton></a>
            </div>
            <div class="w-full py-2 flex justify-between items-center">
              <p>{{ $t('common.modals.autopayPayPal') }}</p>
              <div id="paypal-subscription" />
            </div>
          </template>

          <AppDivider v-if="order.paypal_invoice_link" />

          <div class="w-full h-fit flex justify-between items-center">
            <p>{{ $t('common.modals.cancelSubscription') }}</p>
            <button
              class="w-fit text-brand-black active:text-brand-red cursor-pointer underline underline-offset-8"
              @click="handleCancel"
            >
              {{ $t('common.actions.cancel') }}
            </button>
          </div>
        </div>
      </div>
    </div>
  </n-modal>
</template>
<script setup lang="ts">
import { loadScript } from '@paypal/paypal-js'
import { NModal } from 'naive-ui'
import { onMounted, type Ref, ref } from 'vue'

import AppDivider from '@/components/AppDivider.vue'
import { useCartStore } from '@/store/cart'
import { useUserStore } from '@/store/user'

const props = defineProps<{
  isOpen: boolean
  // eslint-disable-next-line vue/no-unused-properties
  order: any
}>()

const emit = defineEmits(['close', 'statusCancel'])

const paypal: Ref<any> = ref(null)

const cart = useCartStore()
const user = useUserStore()
const config = useAppConfig()

onMounted(async () => {
  try {
    paypal.value = await loadScript({
      clientId: config.paypal.clientId,
      disableFunding: 'credit,card',
      currency: 'EUR',
      vault: 'true',
      intent: 'subscription',
    })
    if (paypal.value) {
      await paypal.value
        .Buttons({
          createSubscription() {
            return cart
              .makeRecurring(props?.order.id)
              .then((data: any) => data.id)
          },
          onApprove() {},
        })
        .render('#paypal-subscription')
    }
  } catch (err: any) {
    console.error(err)
  }
})

const handleCancel = () => {
  cart.cancelRecurring(props?.order.id)
  user.getOrdersByVisitors()
  emit('statusCancel')
}
</script>
