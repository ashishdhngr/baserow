<template>
  <Context ref="context">
    <ul class="context__menu">
      <li v-if="hasValidExporter">
        <a @click="exportView()">
          <i class="context__menu-icon fas fa-fw fa-file-export"></i>
          {{ $t('viewContext.exportView') }}
        </a>
      </li>
      <li>
        <a @click="openWebhookModal()">
          <i class="context__menu-icon fas fa-fw fa-globe"></i>
          {{ $t('viewContext.webhooks') }}
        </a>
      </li>
      <li>
        <a @click="enableRename()">
          <i class="context__menu-icon fas fa-fw fa-pen"></i>
          {{ $t('viewContext.renameView') }}
        </a>
      </li>
      <li>
        <a @click="deleteView()">
          <i class="context__menu-icon fas fa-fw fa-trash"></i>
          {{ $t('viewContext.deleteView') }}
        </a>
      </li>
    </ul>
    <DeleteViewModal ref="deleteViewModal" :view="view" />
    <ExportTableModal ref="exportViewModal" :table="table" :view="view" />
    <WebhookModal ref="webhookModal" :table="table" />
  </Context>
</template>

<script>
import context from '@baserow/modules/core/mixins/context'
import viewTypeHasExporterTypes from '@baserow/modules/database/utils/viewTypeHasExporterTypes'

import ExportTableModal from '@baserow/modules/database/components/export/ExportTableModal'
import DeleteViewModal from './DeleteViewModal'
import WebhookModal from '@baserow/modules/database/components/webhook/WebhookModal.vue'

export default {
  name: 'ViewContext',
  components: { DeleteViewModal, ExportTableModal, WebhookModal },
  mixins: [context],
  props: {
    view: {
      type: Object,
      required: true,
    },
    table: {
      type: Object,
      required: true,
    },
  },
  computed: {
    hasValidExporter() {
      return viewTypeHasExporterTypes(this.view.type, this.$registry)
    },
  },
  methods: {
    setLoading(view, value) {
      this.$store.dispatch('view/setItemLoading', { view, value })
    },
    enableRename() {
      this.$refs.context.hide()
      this.$emit('enable-rename')
    },
    deleteView() {
      this.$refs.context.hide()
      this.$refs.deleteViewModal.show()
    },
    exportView() {
      this.$refs.context.hide()
      this.$refs.exportViewModal.show()
    },
    openWebhookModal() {
      this.$refs.context.hide()
      this.$refs.webhookModal.show()
    },
  },
}
</script>
