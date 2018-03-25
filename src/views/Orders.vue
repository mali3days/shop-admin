<template>
  <div class="animated fadeIn">

    <b-row v-if="items.length > 0">
      <b-col sm="12">
        <c-table
          hover
          striped
          bordered
          small
          fixed
          caption="<i class='fa fa-align-justify'></i> Все заказы"
          :items="items"
          :fields="fields"
        >
        </c-table>
      </b-col>
    </b-row>

    <b-row v-if="items.length === 0">
      <b-col sm="12">
        <h2>Заказов еще нет :(</h2>
      </b-col>
    </b-row>

  </div>

</template>

<script>
import axios from 'axios'
import cTable from './components/Table.vue'

export default {
  name: 'orders',
  components: { cTable },
  data: () => {
    return {
      items: [],
      fields: [
        {
          key: '_id',
          label: 'ID заказа',
          sortable: true
        },
        {
          key: 'status',
          label: 'Статус заказа',
          sortable: true
        },
        {
          key: 'created',
          label: 'Дата поступления заказа',
          sortable: true,
          formatter: (date) => { return new Date(date).toLocaleString('ru') }
        },
        {
          key: 'actions',
          label: 'Действия',
          thStyle: 'width: 120px'
        }
      ]
    }
  },
  created () {
    const token = JSON.parse(localStorage.getItem('tokenShop'))
    const config = {
      headers: { 'Authorization': `Bearer ${token}` }
    }
    axios.get('http://localhost:4040/api/orders', config)
      .then(response => {
        console.log(response.data)

        this.items = response.data
      })
      .catch(function (error) {
        console.log(error)
      })
  }
}
</script>
