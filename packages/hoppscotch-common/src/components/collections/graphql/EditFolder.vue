<template>
  <HoppSmartModal
    v-if="show"
    dialog
    :title="`${t('folder.edit')}`"
    @close="$emit('hide-modal')"
  >
    <template #body>
      <HoppSmartInput
        v-model="name"
        placeholder=" "
        :label="t('action.label')"
        input-styles="floating-input"
        @submit="editFolder"
      />
    </template>
    <template #footer>
      <span class="flex space-x-2">
        <HoppButtonPrimary
          :label="`${t('action.save')}`"
          outline
          @click="editFolder"
        />
        <HoppButtonSecondary
          :label="`${t('action.cancel')}`"
          outline
          filled
          @click="hideModal"
        />
      </span>
    </template>
  </HoppSmartModal>
</template>

<script lang="ts">
import { defineComponent } from "vue"
import { useI18n } from "@composables/i18n"
import { useToast } from "@composables/toast"
import { editGraphqlFolder } from "~/newstore/collections"

export default defineComponent({
  props: {
    show: Boolean,
    folder: { type: Object, default: () => ({}) },
    folderPath: { type: String, default: null },
    editingFolderName: { type: String, default: null },
  },
  emits: ["hide-modal"],
  setup() {
    return {
      toast: useToast(),
      t: useI18n(),
    }
  },
  data() {
    return {
      name: "",
    }
  },
  watch: {
    editingFolderName(val) {
      this.name = val
    },
  },
  methods: {
    editFolder() {
      if (!this.name) {
        this.toast.error(`${this.t("collection.invalid_name")}`)
        return
      }
      editGraphqlFolder(this.folderPath, {
        ...(this.folder as any),
        name: this.name,
      })
      this.hideModal()
    },
    hideModal() {
      this.name = ""
      this.$emit("hide-modal")
    },
  },
})
</script>
