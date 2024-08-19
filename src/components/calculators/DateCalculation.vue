<template>
	<div class="p-5 w-75 m-auto">
		<select v-model="type" class="form-select bg-success p-3 fs-3">
			<option value="difference">Difference Between Date</option>
			<option value="operations">Add or Substract Days</option>
		</select>
		<br />
		<div class="form-group mt-3">
    		<label>from</label>
    		<input type="date" v-model="from" class="form-control fs-3 bg-success" />
  		</div>
		<div v-if="type == 'difference'">
			<div class="form-group mt-2">
				<label>to</label>
				<input type="date" v-model="to" class="form-control fs-3 bg-success" />
			</div>
			<br>
			<h6 class="fs-3">Difference</h6>
			<h5 class="font-weight-bold" style="font-size:3rem">{{ difference }}</h5>
			<h5 style="font-size: 2rem;" class="m-3">{{ displayed_difference }}</h5>
		</div>
		<div v-if="type == 'operations'">
			<div class="row justify-content-around mb-2">
				<div class="col-5">
					<div class="custom-control custom-radio custom-control-inline">
						<input type="radio" class="custom-control-input" v-model="operation.type" :id="`${id}_date_calculation_operation_1`" value="add">
						<label class="custom-control-label" :for="`${id}_date_calculation_operation_1`">add</label>
					</div>
				</div>
				<div class="col-5">
					<div class="custom-control custom-radio custom-control-inline">
						<input type="radio" class="custom-control-input" v-model="operation.type" :id="`${id}_date_calculation_operation_2`" value="substract">
						<label class="custom-control-label" :for="`${id}_date_calculation_operation_2`">substract</label>
					</div>
				</div>
			</div>
			<div class="row mt-2">
				<div class="col-4">
					<div class="form-group">
						<label>years</label>
						<input type="number" min="0" v-model="operation.years" class="form-control" />
					</div>
				</div>
				<div class="col-4">
					<div class="form-group">
						<label>months</label>
						<input type="number" min="0" v-model="operation.months" class="form-control" />
					</div>
				</div>
				<div class="col-4">
					<div class="form-group">
						<label>days</label>
						<input type="number" min="0" v-model="operation.days" class="form-control" />
					</div>
				</div>
			</div>
			<br>
			<h6>date</h6>
			<h5 class="font-weight-bold">{{ date_result }}</h5>
			<h5>{{ displayed_date_result }}</h5>
		</div>
	</div>
</template>

<script>
import vac from '../../mixins/vac'
export default {
	name: 'DateCalculation',
	mixins: [vac],
	data: () => ({
		type: 'difference',
		from: currentDate(),
		to: currentDate(),
		operation: {
			type: 'add',
			years: 0,
			months: 0,
			days: 0,
		}
	}),
	computed: {
		/**
		 * Difference en heure
		 */
		difference_dates() {
			const date1 = (new Date(this.from)).getTime();
         	const date2 = (new Date(this.to)).getTime();
         
		 	if (date1 > date2) {
				return date1 - date2;
			 }
		 	return date2 - date1;
        },
		/**
		 * Difference en jours
		 */
		difference_days() {
			return this.difference_dates / (1000 * 3600 * 24);
		},
		/**
		 * Difference global
		 */
		difference() {
			let diff = this.difference_days;

			let result = [];

			let diffYear = parseInt(diff / 365);
			diff = diff % 365;
			if (diffYear > 0) {
				let res = `${diffYear} `;
				res += (diffYear > 1) ? this.__vac_translate('years') :  this.__vac_translate('year');
				result.push(res);
			}

			let diffMonth = parseInt(diff / 30);
			diff = diff % 30;
			if (diffMonth > 0) {
				let res = `${diffMonth} `;
				res += (diffMonth > 1) ? this.__vac_translate('months') :  this.__vac_translate('month');
				result.push(res);
			}

			let diffWeek = parseInt(diff / 7);
			diff = diff % 7;
			if (diffWeek > 0) {
				let res = `${diffWeek} `;
				res += (diffWeek > 1) ? this.__vac_translate('weeks') :  this.__vac_translate('week');
				result.push(res);
			}

			if (diff > 0) {
				let res = `${diff} `;
				res += (diff > 1) ? this.__vac_translate('days') :  this.__vac_translate('day');
				result.push(res);
			}

			return result.join('; ').toLowerCase();
		},
		displayed_difference() {
			if (this.difference_days == 0) {
				return this.__vac_translate('identics_dates');
			}
			if (this.difference_days == 1) {
				return `1 ${this.__vac_translate('day').toLowerCase()}`;
			}
			return `${this.difference_days} ${this.__vac_translate('days').toLowerCase()}`;
		},

		/**
		 * Resultat de la date après une option d'ajout ou de soustraction
		 */
		date_operation() {
			let current = new Date(this.from);
			
			const days = parseInt(this.operation.days);
			const months = parseInt(this.operation.months);
			const years = parseInt(this.operation.years);

			if (this.operation.type == 'add') {
				current.setDate(current.getDate() + days);
				current.setMonth(current.getMonth() + months);
				current.setFullYear(current.getFullYear() + years);
			}
			if (this.operation.type == 'substract') {
				current.setDate(current.getDate() - days);
				current.setFullYear(current.getFullYear() - years);
				current.setMonth(current.getMonth() - months);
			}

			return current;
		},
		date_result() {
			return this.date_operation.toLocaleDateString();
		},
		displayed_date_result() {
			return this.date_operation.toLocaleDateString(undefined, { weekday: 'long', year: 'numeric', month: 'long', day: 'numeric' });
		}
	}
}

const currentDate = () => {
	const today = new Date();

	let month = today.getMonth() + 1;
	if (month < 10) {
		month = `0${month}`;
	}

	let day = today.getDate();
	if (day < 10) {
		day = `0${day}`;
	}

	return `${today.getFullYear()}-${month}-${day}`;
}
</script>