<template>
   <div>
	<cnc-axes-position :machine-position="machinePosition" @click="machinePosition = !machinePosition" />
	<div class="center-container"  ref="jogContainer"></div>
   </div>
</template>

<script>
'use strict';
import jogControl from './JogControl.js';
import { mapState, mapActions } from 'vuex';
export default {
	data() {
		return {
			machinePosition: false
		}
	},
	computed: {
		...mapState('settings', ['darkTheme']),
	},
	mounted() {
		this.$options.jogControl = new jogControl(this.$refs.jogContainer, this.jogAction);
		this.$options.jogControl.config.noSelectClass = 'noselect';
		this.$options.jogControl.updateTheme(this.darkTheme);
		this.$options.jogControl.render();

		//watch for resizing events
		window.addEventListener('resize', () => {
			this.$nextTick(() => {
				this.resize();
			});
		});
	},
	methods: {
		...mapActions('machine', ['sendCode']),
		jogAction(command) {
			this.sendCode(command);
		},
		resize() {
         this.$refs.jogContainer.style.height = window.innerHeight - document.getElementById('global-container').clientHeight - document.getElementsByClassName('v-toolbar__content')[0].clientHeight - 22 + 'px';
         this.$options.jogControl.render();
         console.log(this.$refs.jogContainer.style.height);
		},
	},
	watch: {
		darkTheme: function (to) {
			this.$options.jogControl.updateTheme(to);
		},
	},
};
</script>

<style>

.center-container{
   text-align:center;
   height:80%;
   width:100%;
   
}
.noselect {
	-webkit-touch-callout: none;
	-webkit-user-select: none;
	-khtml-user-select: none;
	-moz-user-select: none;
	-ms-user-select: none;
	user-select: none;
}
</style>  
