<template>
  <table class="rounded-lg m-5 w-5/6 mx-auto bg-gray-800 text-gray-200">
		<tr class="text-center">
			<th class="px-4 py-3" colspan=4>Pickup Orders</th>
		</tr>
    <tr class="text-left border-b border-gray-300">
      <th class="px-4 py-3">Order</th>
      <th class="px-4 py-3">Name</th>
      <th class="px-4 py-3">Status</th>
      <th class="px-4 py-3">Ready In</th>
    </tr>
    
    <OrderTableRow
      v-for="order in pagedOrders()"
      :key="order.order_id"
      :orderNumber="order.order_id"
      :customerName="order.customer_name"
      :orderStatus="order.order_status"
      :readyAt="order.ready_at"
    />

    <tfoot class="text-right text-lg">
      <tr>
        <td colspan="4" class="px-4 py-3">Current Wait Time: {{ eta }}</td>
      </tr>
    </tfoot>
  </table>
</template>

<script lang="ts">
import {defineComponent} from 'vue';
import OrderTableRow from './OrderTableRow.vue';

export default defineComponent({
  name: 'OrderTable',
  components: {
    OrderTableRow,
  },
	props: {
		orders: {
			required: true,
		},
    page: {
      required: true,
      type: Number,
    },
    perPage: {
      required: true,
      type: Number,
    },
    eta: {
      required: true,
      type: String,
    }
	},
  methods: {
    pagedOrders() {
      let start = this.page * this.perPage
      return this.orders.slice(start, start + this.perPage)
    },
  }
});
</script>
