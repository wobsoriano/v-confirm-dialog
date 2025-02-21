<script setup lang="ts">
import { VBtn, VSnackbar, VThemeProvider } from 'vuetify/components'
import { type Component, type PropType, computed, ref } from 'vue'

const props = defineProps({
  text: {
    type: String,
    required: false,
    default: '',
  },
  contentComponent: {
    type: Object as PropType<Component>,
    required: false,
  },
  snackbarProps: {
    type: Object,
    required: false,
    default: () => ({}),
  },
  showCloseButton: {
    type: Boolean,
    required: false,
    default: true,
  },
  closeButtonProps: {
    type: Object,
    required: false,
    default: () => ({}),
  },
  closeButtonText: {
    type: String,
    required: false,
    default: 'Close',
  },
  theme: {
    type: String,
    required: true,
  },
})

const emit = defineEmits(['close'])

const snackbar = ref(true)

const finalSnackbarProps = computed(() => {
  return {
    ...props.snackbarProps,
    onAfterLeave() {
      props.snackbarProps.onAfterLeave?.()
      emit('close')
    },
  }
})
</script>

<template>
  <VThemeProvider :theme="theme">
    <VSnackbar v-bind="finalSnackbarProps" v-model="snackbar">
      <template v-if="contentComponent">
        <Component :is="contentComponent" />
      </template>
      <template v-else>
        {{ text }}
      </template>
      <template v-if="showCloseButton" #actions>
        <VBtn variant="text" :text="closeButtonText" v-bind="closeButtonProps" @click="snackbar = false" />
      </template>
    </VSnackbar>
  </VThemeProvider>
</template>
