<template>
  <n-modal :show="isOpen" @mask-click="$emit('close')">
    <div
      class="fixed w-[70%] h-[70%] bg-white rounded-md top-1/2 left-1/2 -translate-x-2/4 -translate-y-2/4 overflow-y-auto"
    >
      <div class="flex flex-col">
        <div
          class="flex justify-end items-center border-b-[1px] border-brand-black py-[16px]"
        >
          <div
            class="flex gap-[20px] items-center mr-10"
            @click="$emit('close')"
          >
            {{ $t('common.actions.close') }}
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

        <div class="flex flex-col gap-10 m-10">
          <div>
            <h1 class="font-medium text-4xl mb-10">
              {{ $t('common.modals.calendar') }}
            </h1>

            <form
              ref="form"
              class="flex flex-col gap-2 mt-10 relative"
              @submit.prevent="sendModalCourse"
            >
              <n-config-provider
                :locale="naiveLocale[locale].locale"
                :date-locale="naiveLocale[locale].date"
              >
                <n-calendar
                  v-model:value="value"
                  #="{ year, month, date }"
                  @update:value="handleUpdateValue"
                >
                  {{ date }}.{{ month }}.{{ year }}
                </n-calendar>
              </n-config-provider>

              <div class="grid grid-cols-1 lg:grid-cols-2 gap-[12px]">
                <AppInput
                  v-model="registrationForm.first_name"
                  :placeholder="$t('cart.registerDetails.name')"
                  required
                  pattern=".{1,64}"
                  title="The name must be from 1 to 64 characters"
                  @blur="checkValidity"
                />
                <AppInput
                  v-model="registrationForm.last_name"
                  :placeholder="$t('cart.registerDetails.surname')"
                  required
                  pattern=".{1,64}"
                  title="Last name must be from 1 to 64 characters"
                  @blur="checkValidity"
                />
              </div>

              <div class="grid grid-cols-1 lg:grid-cols-2 gap-[12px]">
                <AppInput
                  v-model="registrationForm.email"
                  :placeholder="$t('cart.registerDetails.email')"
                  type="email"
                  required
                  @blur="checkValidity"
                />
                <AppInput
                  v-model="registrationForm.phone"
                  placeholder="+491112221212"
                  pattern="^\+\d{8,15}$"
                  type="tel"
                  required
                  @blur="checkValidity"
                />
              </div>

              <AppTextarea
                v-model="registrationForm.comment"
                :placeholder="$t('common.modals.additionalInfo')"
                type="text"
                required
                @blur="checkValidity"
              />

              <div class="w-full mt-5 flex gap-4">
                <NCheckbox v-model:checked="checkbox" required class="pt-1" />
                <p>
                  {{ $t('common.modals.registerInService') }}
                  <NuxtLink
                    to="/legal"
                    class="underline underline-offset-8 cursor-pointer text-brand-black hover:text-brand-red"
                  >
                    {{ $t('common.modals.termsOfAgreement') }}
                  </NuxtLink>
                </p>
              </div>

              <AppButton
                class="w-full mt-10"
                type="submit"
                :disabled="!(form?.checkValidity() && checkbox)"
              >
                {{ $t('common.actions.send') }}
              </AppButton>
            </form>
          </div>
        </div>
      </div>
    </div>
  </n-modal>
</template>
<script setup lang="ts">
import {
  dateDeDE,
  dateEnUS,
  dateRuRU,
  deDE,
  enUS,
  NCalendar,
  NCheckbox,
  NConfigProvider,
  NModal,
  ruRU,
} from 'naive-ui'
import { ref, type VNodeRef } from 'vue'

import { useUserStore } from '@/store/user'

defineProps<{
  isOpen: boolean
}>()
// Init component
const emit = defineEmits(['close'])

const { locale } = useI18n({ useScope: 'global' })

// State
const value = ref(new Date())
const checkbox = ref(false)

const user = useUserStore()

// eslint-disable-next-line @typescript-eslint/no-shadow
const handleUpdateValue = (value: Date) => {
  value.value = value
}

const registrationForm = ref({
  first_name: '',
  last_name: '',
  email: '',
  phone: '',
  comment: '',
})

const checkValidity = (event: {
  target: { reportValidity: () => void }
  relatedTarget: { focus: () => void }
}) => {
  event?.target?.reportValidity()
  event?.relatedTarget?.focus()
}

const naiveLocale = {
  ru: {
    locale: ruRU,
    date: dateRuRU,
  },
  en: {
    locale: enUS,
    date: dateEnUS,
  },
  de: {
    locale: deDE,
    date: dateDeDE,
  },
}

const sendModalCourse = () => {
  user.postAppointment({
    ...registrationForm.value,
    appointment_date: new Date(value.value).toISOString().slice(0, 10),
  })
  emit('close')
}

const form = ref<VNodeRef | undefined>(undefined)
</script>
