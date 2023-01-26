<template >
	<div class="my-table-row">
		<div class="row row__city">{{ row_data.city }}</div>
		<div class="row row__min-temp">{{ row_data.daily.temperature_2m_min[0] }}<span>{{
			row_data.daily_units.temperature_2m_min[0]
		}}</span></div>
		<div class="row row__max-temp">{{ row_data.daily.temperature_2m_max[0] }}<span>{{
			row_data.daily_units.temperature_2m_min[0]
		}}</span></div>
		<my-button class="my-button_d" @click="$emit('remove', row_data)">Delete</my-button>
	</div>
</template>
<script>

export default {
	name: 'my-table-row',
	props: {
		row_data: {
			type: Object,
		}
	},
	data() {
		return {

		}
	},
	computed: {

	}


}
</script>
