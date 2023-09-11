<template>
	<view class="content">
		<view class="fullscreen">
			<QRCodeScanner></QRCodeScanner>
		</view>
	</view>
</template>

<script>
	import QRCodeScanner from "../../components/QRCodeScannerWeb.vue";
	export default {
		components: {
			QRCodeScanner
		},
		data() {
			return {
				title: 'Hello',
			}
		},
		onLoad() {

		},
		methods: {

		}
	}
</script>

<style>
	.content {
		display: flex;
		flex-direction: column;
		align-items: center;
		justify-content: center;
	}

	.fullscreen {
    position: absolute;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
  }
</style>
