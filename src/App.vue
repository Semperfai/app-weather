<template>
	<div class="app">
		<div class="app__picker"><date-picker v-model:value="time0"></date-picker>
			<my-button @click="changeDate">Change date!</my-button>
		</div>
		<my-table class="app__table" @remove="removeRow" :cities_data="cities" />
	</div>
</template>

<script>
import DatePicker from 'vue-datepicker-next';
import 'vue-datepicker-next/index.css';
import MyTable from '@/components/MyTable.vue';
import axios from "axios"
export default {
	name: 'app',
	components: {
		MyTable, DatePicker
	},
	data() {
		return {
			time0: null,
			coordinates: [
				{ city: 'Paris', latitude: 48.85, longitude: 2.35 },
				{ city: 'Berlin', latitude: 52.52, longitude: 13.41 },
				{ city: 'London', latitude: 51.51, longitude: -0.13 },
			],
			cities: [],
			date: {
				year: '',
				month: '',
				day: ''
			}
		}
	},
	computed: {

	},
	methods: {
		removeRow(row) {
			this.cities = this.cities.filter((c) => c.city !== row.city)
		},
		pickerNow() {
			this.time0 = new Date(Date.now())
		},
		async GET_CITIES_FROM_API() {
			try {
				for (let i = 0; i < this.coordinates.length; i++) {
					let response = await axios.get(`https://api.open-meteo.com/v1/forecast?latitude=${this.coordinates[i].latitude}&longitude=${this.coordinates[i].longitude}&hourly=temperature_2m&daily=temperature_2m_max,temperature_2m_min&timezone=Europe%2FBerlin&start_date=2023-01-25&end_date=2023-01-25`, {
						method: 'GET'
					})
					response.data.city = this.coordinates[i].city
					this.cities = [...this.cities, response.data]
				}
			} catch (e) {
				console.log(e);
			}
		},
		async changeDate() {
			try {
				for (let i = 0; i < this.coordinates.length; i++) {
					let response = await axios.get(`https://api.open-meteo.com/v1/forecast?latitude=${this.coordinates[i].latitude}&longitude=${this.coordinates[i].longitude}&hourly=temperature_2m&daily=temperature_2m_max,temperature_2m_min&timezone=Europe%2FBerlin&start_date=${this.changeYear}-${this.changedMonth}-${this.changeDay}&end_date=${this.changeYear}-${this.changedMonth}-${this.changeDay}`, {
						method: 'GET'
					})
					response.data.city = this.coordinates[i].city
					this.cities = [...this.cities, response.data].slice(-this.cities.length)
				}
			} catch (e) {
				console.log(e);
			}

		},

	},
	computed: {
		changeYear() {
			let newDate = new Date(this.time0)
			return this.date.year = `${newDate.getFullYear()}`
		},
		changedMonth() {
			let newDate = new Date(this.time0)
			this.date.year = `${newDate.getFullYear()}`
			if (newDate.getMonth() + 1 < 10) {
				return this.date.month = `0${newDate.getMonth() + 1}`
			} else {
				return this.date.month = `${newDate.getMonth() + 1}`
			}
		},
		changeDay() {
			let newDate = new Date(this.time0)
			if (newDate.getDate() < 10) {
				return this.date.day = `0${newDate.getDate()}`
			} else {
				return this.date.day = `${newDate.getDate()}`
			}
		}
	},
	mounted() {
		this.GET_CITIES_FROM_API()
		this.pickerNow()
	}


}
</script>

<style lang="scss">

</style>
