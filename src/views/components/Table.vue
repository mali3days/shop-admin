
<template>
  <b-card :header="caption">
    <b-table :hover="hover" :striped="striped" :bordered="bordered" :small="small" :fixed="fixed" class="table-responsive-sm" :items="items" :fields="fields" :current-page="currentPage" :per-page="perPage">
      <template slot="actions" slot-scope="row" class="col-md-2">
        <b-button @click.stop="info(row.item, row.index, $event.target)" class="mr-1">
          Просмотреть
        </b-button>
      </template>
    </b-table>
    <nav>
      <b-pagination :total-rows="getRowCount(items)" :per-page="perPage" v-model="currentPage" prev-text="Prev" next-text="Next" hide-goto-end-buttons/>
    </nav>
  </b-card>
</template>


<script>
export default {
  name: 'table',
  props: {
    caption: {
      type: String,
      default: 'Table'
    },
    hover: {
      type: Boolean,
      default: false
    },
    striped: {
      type: Boolean,
      default: false
    },
    bordered: {
      type: Boolean,
      default: false
    },
    small: {
      type: Boolean,
      default: false
    },
    fixed: {
      type: Boolean,
      default: false
    },
    items: {
      type: Array,
      default: []
    },
    fields: {
      type: Array,
      default: []
    }
  },
  data: () => {
    return {
      currentPage: 1,
      perPage: 10,
      totalRows: 0
    }
  },
  methods: {
    getBadge (status) {
      return status === 'Active'
        ? 'success'
        : status === 'Inactive'
          ? 'secondary'
          : status === 'Pending'
            ? 'warning'
            : status === 'Banned' ? 'danger' : 'primary'
    },
    getRowCount (items) {
      return items.length
    },
    info (order, index, button) {
      this.$router.push({
        path: `orders/${order._id}`,
        params: { orderId: order._id }
      })
    }
  }
}
</script>
