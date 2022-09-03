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
	@import "uview-ui/index.scss";

	page {
		height: 100%;
	}
	
	.content {
		min-height: 100%;
		background-image: linear-gradient(to bottom, #fff, #f8fafc);
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
