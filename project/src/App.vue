<template>
	<div id="app">
		<div class="wrapper">
			<tableItem />
			<formItem />
		</div>
	</div>
</template>

<script>
	import tableItem from './components/tableItem.vue';
  import formItem from './components/formItem.vue';
  
	import 'bootstrap/dist/css/bootstrap.css';
	import 'bootstrap-vue/dist/bootstrap-vue.css';

	export default {
		name: 'App',
		components: {
			tableItem,
			formItem,
		},
	};
</script>

<style lang="scss">
	.wrapper {
		font-family: Helvetica, Arial, sans-serif;
		-webkit-font-smoothing: antialiased;
		-moz-osx-font-smoothing: grayscale;
    position: fixed;
    top: 0;
    left: 0;
    right: 0;
    bottom: 0;
    display: flex;
    align-items: center;
    justify-content: center;
    flex-direction: column;
    padding-left: 20px;
    padding-right: 20px;

    section {
      width: 100%;
    }

    button {
      border-radius: 20px;
      padding: .3em 1em;
    }
    p {
      margin: 0;
    }
	}
</style>
