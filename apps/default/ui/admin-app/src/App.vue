<script setup lang="ts">
import { ref, onMounted } from 'vue'
import { RouterView } from 'vue-router'
import { LocalStorageHelper } from './models/helper/LocalStorageHelper'
import { OldUISwitcher } from './models/helper/OldUISwitcher'

const API_URL = 'http://localhost/api/config'

const appConfig: any = ref({})
const isLoading = ref(true)

// auto switch only for PROD (localhost)
if (OldUISwitcher.isOldUI() && import.meta.env.PROD) {
  OldUISwitcher.redirectToOldUI()
}

onMounted(async () => {
  appConfig.value = await (await fetch(API_URL)).json()
  isLoading.value = false
  document.title = appConfig.value.config.appName
  LocalStorageHelper.setItem('config', appConfig.value.config)
})
</script>

<template>
  <RouterView v-if="!isLoading" />
</template>
