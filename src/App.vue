<template>
  <div id="app">
    <!-- <div class="row mt-4 "> -->
    <div class="wrapper-manual">
		<!-- <div class="col-3 offset-2 m-auto"> -->
		<div class="container-manual">
			<calculator />
		</div>
	</div>
  </div>
</template>

<script>
import Calculator from './Calculator.vue'

export default {
  name: 'App',
  components: {
    Calculator
  }
}
</script>

