<template>
  <view>
      <camera device-position="back" flash="off" @error="error" style="width: 100%; height: 300px;"></camera>
  </view>
</template>

<script>
  import { onBeforeUnmount, onMounted, ref } from "vue";
  export default {
    name:"QRCodeScannerMP",
    setup(props,context){
      const scanning = ref(false);
      const interval = ref<any>(null);
      const ctx = uni.createCameraContext();
      const startScanning = () => {
        stopScanning();
        interval.value = setInterval(captureAndScan,500);
      }
      const stopScanning = () => {
        clearInterval(interval.value);
      }
      
      const decode = async (base64) =>  {
        uni.request({
            url: 'https://barcode-reading-server.vercel.app/',
            method: 'POST',
            data: {
                base64: base64
            },
            header: {
              'content-type': 'application/json'
            },
            success: (res) => {
              console.log(res.data);
              let results = [];
              let parsedResults = res.data.results;
              for (var i = 0; i < parsedResults.length; i++) {
                let parsedResult = parsedResults[i];
                let result = {};
                result.barcodeText = parsedResult.barcodeText;
                result.barcodeFormatString = parsedResult.barcodeFormat;
                result.localizationResult = {};
                result.localizationResult.x1 = parsedResult.x1;
                result.localizationResult.x2 = parsedResult.x2;
                result.localizationResult.x3 = parsedResult.x3;
                result.localizationResult.x4 = parsedResult.x4;
                result.localizationResult.y1 = parsedResult.y1;
                result.localizationResult.y2 = parsedResult.y2;
                result.localizationResult.y3 = parsedResult.y3;
                result.localizationResult.y4 = parsedResult.y4;
                results.push(result);
              }
              context.emit("scanned", results);
            },
            complete: (res) => {
              scanning.value = false;
            }
        });
      }
      
      const captureAndScan = () => {
        if (scanning.value === true) {
          console.log("skip");
          return;
        }
        scanning.value === true;
        ctx.takePhoto({
          quality: 'high',
          success: (res) => {
            let url = res.tempImagePath;
            uni.getFileSystemManager().readFile({
                filePath: url,
                encoding: 'base64', 
                success: res => {
                  let base64 = res.data 
                  decode(base64);
                },fail: (e) => {
                  console.log("Failed");
                  scanning.value = false;
                },
              }
            )
          },
          fail: ()=> {
            scanning.value = false;
          }
        });
      }
      
      onMounted(async () => {
        startScanning();
      });
      onBeforeUnmount(async () => {
        stopScanning();
      });
    },
    data() {
      return {
        
      };
    }
  }
</script>

<style>

</style>