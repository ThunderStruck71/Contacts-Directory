<template>
  <div class="table-container">
    <vue-ads-table
      :columns="columns"
      :rows="contacts"
      :classes="classes"
    >
    </vue-ads-table>
  </div>
</template>

<script>
import '../../../node_modules/@fortawesome/fontawesome-free/css/all.css'
import '../../../node_modules/vue-ads-table-tree/dist/vue-ads-table-tree.css'
import {VueAdsTable} from 'vue-ads-table-tree'

export default {
  name: 'ContactsTable',
  props: ['contacts'],
  data () {
    const columns = [
      {
        property: 'name',
        title: 'Имя',
        direction: null,
        collapseIcon: true
      },
      {
        property: 'phone',
        title: 'Телефон',
        direction: null
      }
    ]

    const classes = {
      table: {
        'vue-ads-font-sans': true,
        'vue-ads-border': true,
        'vue-ads-pb-1': true
      },
      'all/': {
        'hover:vue-ads-bg-gray-200': true,
        'word-break-all': true
      },
      'all/all': {
        'vue-ads-px-4': true,
        'vue-ads-py-2': true,
        'vue-ads-text-sm': true
      },
      'even/': {
        'vue-ads-bg-gray-100': true
      },
      'odd/': {
        'vue-ads-bg-white': true
      },
      '0/': {
        'vue-ads-bg-gray-100': false,
        'hover:vue-ads-bg-gray-200': false
      },
      '0/all': {
        'vue-ads-px-4': true,
        'vue-ads-py-2': true,
        'vue-ads-text-left': true
      },
      '0_-1/': {
        'vue-ads-border-b': true
      },
      '/0_-1': {
        'vue-ads-border-r': true
      },
      '0/0_-1': {
        'vue-ads-w-2/4': true
      },
      '0/1_': {
        'vue-ads-w-2/4': true
      }
    }

    return {
      page: 0,
      slottedColumns: ['name', 'phone'],
      columns,
      classes
    }
  },
  components: {
    VueAdsTable
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style>
.table-container {
  width: 50%;
  margin-bottom: 10px;
  margin-right: 30px;
}

.word-break-all {
  word-break: break-all;
}

.vue-ads-flex {
  display: flex;
  flex-wrap: wrap;
  align-items: center;
}
</style>
