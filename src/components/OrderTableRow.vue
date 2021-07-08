<template>
  <tr class="bg-gray-700 border-b border-gray-600 text-left"
    :class="{ 'bg-green-700': completed() }"
  >
    <td class="px-4 py-4">{{ orderNumber }}</td>
    <td class="px-4 py-4">{{ customerName }}</td>
    <td class="px-4 py-4">{{ order_status_for_humans() }}</td>
    <td class="px-4 py-4">{{ expected_time() }}</td>
  </tr>
</template>

<script lang="ts">
import {defineComponent} from 'vue';

export default defineComponent({
  name: 'OrderTableRow',
  props: {
    orderNumber: {
      type: Number,
      required: true,
    },
    customerName: {
      type: String,
      required: true,
    },
    orderStatus: {
      type: String,
    },
    readyAt: {
      type: String,
    },
  },
  methods: {
    ready_in_minutes() {
	    var ready_at = new Date(this.readyAt)
			var ready_in = ready_at - new Date()
			ready_in = Math.round(ready_in / (1000 * 60))
      ready_in -= 60 // timezones lol

			return ready_in
    },
    completed() {
      if (this.orderStatus == 'completed') {
        return true
      }
      return false
    },
    incoming() {
      if (this.orderStatus == 'incoming') {
        return true
      }
      return false
    },
    expected_time() {
      if (this.incoming() && this.ready_in_minutes() > 0) {
        return ''
      }
      if (this.completed()) {
        return 'Now'
      }
      if (this.ready_in_minutes() <= 0) {
        return 'Just a sec'
      }
      return this.ready_in_minutes() + ' minutes'
    },
    order_status_for_humans() {
      return {'incoming': 'Receiving Order', 'completed': 'Ready to collect', 'in_kitchen': 'Preparing tasty pizza!'}[this.orderStatus]
    }
  },
});
</script>
