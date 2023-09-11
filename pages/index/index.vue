<template>
	<view class="content">
    <button @click="startScan">Start Scanning</button>
		<view class="fullscreen" v-if="scanning">
			<QRCodeScanner @scanned="scanned"></QRCodeScanner>
		</view>
    <view v-for="(result,index) in barcodeResults">
      <view>{{ (index+1) + ". " + result.barcodeFormatString + ": " + result.barcodeText }}</view>
    </view>
	</view>
</template>

<script>
	import QRCodeScanner from "../../components/QRCodeScannerWeb.vue";
  import { ref } from "vue";
	export default {
		components: {
			QRCodeScanner
		},
    setup(){
      const scanning = ref(false);
      const barcodeResults = ref([]);
      const startScan = () => {
        scanning.value = true;
      }
      const scanned = (results) => {
        if (results.length>0) {
          barcodeResults.value = results;
          scanning.value = false;
        }
      }
      return {
        startScan,
        scanned,
        barcodeResults,
        scanning
      }
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
