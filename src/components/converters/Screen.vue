<template>
	<div class="p-1 w-75 m-auto">
		<div class="row mt-3 p-3 d-flex justify-content-center">
			<div class="col-4">
				<div class="form-group">
					<label style="font-size:2rem;">{{ __vac_translate('initial_unit') }}</label>
					<select v-model="initial_unit" style="font-size:2rem;" class="form-select m-3 p-3 bg-success manual-color">
						<option disabled selected style="font-size:2rem;">{{ __vac_translate('select_the_initial_unit') }}</option>
						<option v-for="(unit, i) in units" :key="i" :value="unit.value" style="font-size:1.6rem;">{{ unit.text }}</option>
					</select>
				</div>
			</div>
			<div class="col-2">
				<div class="form-group d-flex justify-content-center align-items-center">
					<label style="visibility:hidden !important; opacity: 0 !important;">Hide</label>
					<button class="btn btn-light btn-icon btn-sm" @click.prevent="toggleUnits"><i class="fa fa-exchange-alt"></i></button>
				</div>
			</div>
			<div class="col-4">
				<div class="form-group m-2">
					<label style="font-size:2rem;">{{ __vac_translate('final_unit') }}</label>
					<select v-model="final_unit" style="font-size:2rem;" class="form-select m-3 p-3 bg-success manual-color">
						<option disabled selected style="font-size:2rem;">{{ __vac_translate('select_the_final_unit') }}</option>
						<option v-for="(unit, i) in units" :key="i" :value="unit.value" style="font-size:1.6rem;">{{ unit.text }}</option>
					</select>
				</div>
			</div>
		</div>
		<div class="vac-screen manual-converters-screen">
			<div class="w-100 text-right vac-screen-operation">{{ displayed_initial_unit}}</div>
			<div class="w-100 text-right vac-screen-result">{{ current }}</div>
		</div>
		<div class="vac-screen  manual-converters-screen">
			<div class="w-100 text-right vac-screen-operation">{{ displayed_final_unit }}</div>
			<div class="w-100 text-right vac-screen-result">{{ result }}</div>
		</div>
		<div class="col-10 offset-1 px-0">
			<div class="d-flex justify-content-end vac-buttons">
				<button class="btn btn-dark" @click="clear()">CE</button>
				<button class="btn btn-dark" @click="backspace()">&larr; Backspace</button>
			</div>
			<div class="d-flex justify-content-between vac-buttons">
				<button class="btn btn-dark" style="flex: 1" @click="addElement(7)">7</button>
				<button class="btn btn-dark" style="flex: 1" @click="addElement(8)">8</button>
				<button class="btn btn-dark" style="flex: 1" @click="addElement(9)">9</button>
			</div>
			<div class="d-flex justify-content-between vac-buttons">
				<button class="btn btn-dark" style="flex: 1" @click="addElement(4)">4</button>
				<button class="btn btn-dark" style="flex: 1" @click="addElement(5)">5</button>
				<button class="btn btn-dark" style="flex: 1" @click="addElement(6)">6</button>
			</div>
			<div class="d-flex justify-content-between vac-buttons">
				<button class="btn btn-dark" style="flex: 1" @click="addElement(1)">1</button>
				<button class="btn btn-dark" style="flex: 1" @click="addElement(2)">2</button>
				<button class="btn btn-dark" style="flex: 1" @click="addElement(3)">3</button>
			</div>
			<div class="d-flex justify-content-end vac-buttons">
				<div style="flex: 1" class="btn"></div>
				<button class="btn btn-dark" style="flex: 1" @click="addElement(0)">0</button>
				<button class="btn btn-dark" style="flex: 1" @click="addElement('.')">.</button>
			</div>
		</div>
	</div>
</template>

<script>
import converter from '../../mixins/converter'
export default {
	name: 'ScreenConverver',
	mixins: [converter],
	props: {
		units: {type: Array, required: true},
		equivalence: {type: Object, required: true}
	},
	methods: {
		toggleUnits() {
			const tmp = this.final_unit 
			this.final_unit = this.initial_unit
			this.initial_unit = tmp
		}
	}
}
</script>