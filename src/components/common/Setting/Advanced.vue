<script lang="ts" setup>
import { ref } from 'vue'
import { NButton, NInput, NSlider, useMessage } from 'naive-ui'
import { useSettingStore } from '@/store'
import type { SettingsState } from '@/store/modules/settings/helper'
import { t } from '@/locales'

const settingStore = useSettingStore()

const ms = useMessage()

const systemMessage = ref(settingStore.systemMessage ?? '')

// const temperature = ref(settingStore.temperature ?? 0.5)

// const top_p = ref(settingStore.top_p ?? 1)

const presence_penalty = ref(settingStore.presence_penalty ?? 0)
	
const frequency_penalty = ref(settingStore.frequency_penalty ?? 0)

function updateSettings(options: Partial<SettingsState>) {
  settingStore.updateSetting(options)
  ms.success(t('common.success'))
}

function handleReset() {
  settingStore.resetSetting()
  ms.success(t('common.success'))
  window.location.reload()
}
</script>

<template>
  <div class="p-4 space-y-5 min-h-[200px]">
    <div class="space-y-6">
      <div class="flex items-center space-x-4">
        <span class="flex-shrink-0 w-[120px]">{{ $t('setting.role') }}</span>
        <div class="flex-1">
          <NInput v-model:value="systemMessage" type="textarea" :autosize="{ minRows: 1, maxRows: 4 }" />
        </div>
        <NButton size="tiny" text type="primary" @click="updateSettings({ systemMessage })">
          {{ $t('common.save') }}
        </NButton>
      </div>
      <div class="flex items-center space-x-4">
        <span class="flex-shrink-0 w-[120px]">{{ $t('setting.presence_penalty') }} </span>
        <div class="flex-1">
          <NSlider v-model:value="presence_penalty" :max="2" :min="-2" :step="0.1" />
        </div>
        <span>{{ presence_penalty }}</span>
        <NButton size="tiny" text type="primary" @click="updateSettings({ presence_penalty })">
          {{ $t('common.save') }}
        </NButton>
      </div>
      <div class="flex items-center space-x-4">
        <span class="flex-shrink-0 w-[120px]">{{ $t('setting.frequency_penalty') }} </span>
        <div class="flex-1">
          <NSlider v-model:value="frequency_penalty" :max="2" :min="-2" :step="0.1" />
        </div>
        <span>{{ frequency_penalty }}</span>
        <NButton size="tiny" text type="primary" @click="updateSettings({ frequency_penalty })">
          {{ $t('common.save') }}
        </NButton>
      </div>
      <div class="flex items-center space-x-4">
        <span class="flex-shrink-0 w-[120px]">&nbsp;</span>
        <NButton size="small" @click="handleReset">
          {{ $t('common.reset') }}
        </NButton>
      </div>
    </div>
  </div>
</template>
