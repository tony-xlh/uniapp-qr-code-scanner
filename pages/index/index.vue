<template>
	<view class="content">
    <button @click="startScan">Start Scanning</button>
		<view class="fullscreen" v-if="scanning">
			<!--  #ifdef H5 -->
      <QRCodeScannerWeb @scanned="scanned" license="DLS2eyJoYW5kc2hha2VDb2RlIjoiMjAwMDAxLTE2NDk4Mjk3OTI2MzUiLCJvcmdhbml6YXRpb25JRCI6IjIwMDAwMSIsInNlc3Npb25QYXNzd29yZCI6IndTcGR6Vm05WDJrcEQ5YUoifQ=="></QRCodeScannerWeb>
      <!--  #endif -->
      <!--  #ifdef APP -->
      <QRCodeScanner @scanned="scanned" license="DLS2eyJoYW5kc2hha2VDb2RlIjoiMjAwMDAxLTE2NDk4Mjk3OTI2MzUiLCJvcmdhbml6YXRpb25JRCI6IjIwMDAwMSIsInNlc3Npb25QYXNzd29yZCI6IndTcGR6Vm05WDJrcEQ5YUoifQ=="></QRCodeScanner>
      <!--  #endif -->
      <!--  #ifdef MP -->
      <QRCodeScannerMP @scanned="scanned"></QRCodeScannerMP>
      <!--  #endif -->
		</view>
    <view v-for="(result,index) in barcodeResults">
      <view>{{ (index+1) + ". " + result.barcodeFormatString + ": " + result.barcodeText }}</view>
    </view>
	</view>
</template>

<script>
  import QRCodeScannerWeb from "../../components/QRCodeScannerWeb.vue";
  import QRCodeScanner from "../../components/QRCodeScannerWebView.vue";
  import QRCodeScannerMP from "../../components/QRCodeScannerMP.vue";
  import { ref } from "vue";
	export default {
		components: {
			QRCodeScannerWeb,
      QRCodeScanner,
      QRCodeScannerMP
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
