<script>
	export default {
		onLaunch: function() {
			console.log('App Launch')
		},
		onShow: function() {
			console.log('App Show')
		},
		onHide: function() {
			console.log('App Hide')
		}
	}
</script>

<style lang="scss">
	/* 注意要写在第一行，同时给style标签加入lang="scss"属性 */
	@import "uview-ui/index.scss";
	page{
		height: 100vh;
	}
	
	::-webkit-scrollbar {
		display: none;
	}
	
	.flex-col {
		display: flex;
		flex-direction: column;
	}
	
	.flex-row {
		display: flex;
		flex-direction: row;
	}
	
	.flex-fill {
		-webkit-flex: 1;
		flex: 1;
	}
</style>
