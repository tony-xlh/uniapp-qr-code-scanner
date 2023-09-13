<template>
  <view>
    <web-view v-if="hasPermission" @message="handlePostMessage" cache="true" :src="url"></web-view>
  </view>
</template>

<script>
  import { onMounted, ref } from "vue";
  export default {
    name:"QRCodeScanner",
    props: ['license'],
    setup(props,context) {
      const url = ref("");
      const hasPermission = ref(false);
      const requestCameraPermission = () => {
        let platform=uni.getSystemInfoSync().platform
        if(platform === 'ios'){
          hasPermission.value = true;
        }else if(platform=='android'){
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
        let newURL = "";
        if (props.license) {
          newURL = "https://tony-xlh.github.io/Vanilla-JS-Barcode-Reader-Demos/uniapp?startScan=true&license=" + encodeURIComponent(props.license);
        }else{
          newURL = "https://tony-xlh.github.io/Vanilla-JS-Barcode-Reader-Demos/uniapp?startScan=true";
        }
        url.value = newURL;
        //#ifdef APP
        requestCameraPermission();
        //#endif
        //#ifdef MP
        hasPermission.value = true;
        //#endif
      })
      return {
        url,
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