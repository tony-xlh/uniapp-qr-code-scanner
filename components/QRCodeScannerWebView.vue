<template>
  <view>
    <web-view v-if="hasPermission" @message="handlePostMessage" src="https://tony-xlh.github.io/Vanilla-JS-Barcode-Reader-Demos/uniapp?startScan=true"></web-view>
  </view>
</template>

<script>
  import { onMounted, ref } from "vue";
  export default {
    name:"QRCodeScanner",
    setup(props,context) {
      const hasPermission = ref(false);
      const requestCameraPermission = () => {
        plus.android.requestPermissions(['android.permission.CAMERA'], function(e){  
          if(e.deniedAlways.length>0){
            console.log(e.deniedAlways.toString());  
          }  
          if(e.deniedPresent.length>0){
            console.log(e.deniedPresent.toString());  
          }  
          if(e.granted.length>0){
            hasPermission.value = true;
          }  
        }, function(e){  
           console.log('Request Permissions error:'+JSON.stringify(e));  
        });  
      }
      const handlePostMessage = (e) => {
        console.log(e);
        if (e.detail.data && e.detail.data.length>0) {
          if (e.detail.data[0].results) {
            context.emit("scanned", JSON.parse(e.detail.data[0].results));
          }
        }
      }
      onMounted(()=>{
        requestCameraPermission();
      })
      return {
        hasPermission,
        handlePostMessage
      }
    },
    data() {
      return {
        
      };
    }
  }
</script>

<style>

</style>