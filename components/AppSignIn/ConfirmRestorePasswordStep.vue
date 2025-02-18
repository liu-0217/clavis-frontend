<template>
  <form ref="form" @submit.prevent="confirmRestore">
    <AppInput
      v-if="!isRestoredPassword"
      v-model="restoreCredentials.password1"
      :placeholder="$t('user.password')"
      type="password"
      class="mt-[12px]"
      required
      pattern="(?=.*\d)(?=.*[a-z])(?=.*[A-Z]).{8,}"
      :title="$t('user.passwordValidation')"
      @blur="checkValidity"
    />
    <AppInput
      v-if="!isRestoredPassword"
      v-model="restoreCredentials.password2"
      :placeholder="$t('user.repeatPassword')"
      type="password"
      class="mt-[12px]"
      required
      pattern="(?=.*\d)(?=.*[a-z])(?=.*[A-Z]).{8,}"
      :title="$t('user.passwordValidation')"
      @blur="checkValidity"
    />

    <p v-if="error" class="text-brand-red mt-2 mb-2">{{ error }}</p>
    <p v-if="isRestoredPassword" class="text-brand-gray mt-2 mb-2">
      {{ $t('user.successRestoreMessage') }}
    </p>

    <AppButton
      v-if="!isRestoredPassword"
      class="block mt-[36px] w-full"
      type="submit"
      :disabled="!form?.checkValidity() ?? false"
    >
      {{ $t('common.actions.restore') }}
    </AppButton>
  </form>
</template>
<script lang="ts" setup>
import { ref, type VNodeRef } from 'vue'
import { useRoute } from 'vue-router'

import { useUserStore } from '../../store/user'
import AppInput from '../AppInput.vue'

// Init component
defineEmits(['close'])

// Store
const userStore = useUserStore()

const { t } = useI18n()

// State
const error = ref('')
const form = ref<VNodeRef | undefined>(undefined)
const isRestoredPassword = ref(false)

const route = useRoute()

const restoreCredentials = ref({
  password1: '',
  password2: '',
})

// Actions
const checkValidity = (event: {
  target: { reportValidity: () => void }
  relatedTarget: { focus: () => void }
}) => {
  event.target.reportValidity()
  event.relatedTarget?.focus()
}

const clearError = () => {
  error.value = ''
}

const confirmRestore = async () => {
  await userStore
    .confirmResetPassword(
      restoreCredentials.value.password1,
      restoreCredentials.value.password2,
      route.query.uid as string,
      route.query.token as string,
    )
    .catch(err => {
      if (Object.keys(err).length !== 0) {
        error.value = t('common.somethingWrong')
        setTimeout(clearError, 2000)
      }
    })
    .then(() => {
      isRestoredPassword.value = true
    })
}
</script>
