<template>
  <div class="main">
    <div class="title">{{page.title}}</div>
    <div class="content">
      <div v-for="(item,index) in page.list" :key="index">
          <div class="ctn_img"  :style="{'background-image':'url('+item.img_src+')'}" ></div>
          <div class="ctn_name" :style="{'margin-bottom': item.desc ?'10px':'30px'}">{{item.name}}</div>
          <div class="ctn_desc" v-if="item.desc">{{item.desc}}</div>
          <a class="ctn_action"  :href="item.htmlUrl">
            <div :style="{backgroundColor: item.color}">
              <span>{{item.action}}</span>
              <span></span>
            </div>
          </a>
      </div>
    </div>
  </div>
  <div class="footer">{{page.footer}}</div>
</template>
<script lang="ts" setup>
import axios from "axios";
import { onMounted, ref, reactive, watch } from "vue";
const page = reactive<any>({
  title:'',
  list:[],
  footer:''
})
const screenWidth = ref<any>()
const getData = () => {
  try{
      axios.get('./static/json/main.json').then(response=>{
          const {data} = response
          page.list = []
          page.desc = data.desc
          page.title = data.title
          page.footer = data.footer
          if( devide.value == 'phone' || devide.value == 'pad' && isOrientation.value) {
            data.cris && page.list.push(data.cris)
            data.elena && page.list.push(data.elena)
            data.andreea && page.list.push(data.andreea)
          }
          if( devide.value == 'pc' || devide.value == 'pad' && !isOrientation.value){
            data.elena && page.list.push(data.elena)
            data.cris && page.list.push(data.cris)
            data.andreea && page.list.push(data.andreea)
          }
          if(devide.value == 'pad'){
            page.list = []
            if(isOrientation.value){ //竖屏
              data.cris && page.list.push(data.cris)
              data.elena && page.list.push(data.elena)
              data.andreea && page.list.push(data.andreea)
            }else{
              data.elena && page.list.push(data.elena)
              data.cris && page.list.push(data.cris)
              data.andreea && page.list.push(data.andreea)
            }
          }
      })
    }catch{}
}
const devide = ref('')
const getOs = () => {
  let os = function () {
			let ua = navigator.userAgent,
				isWindowsPhone = /(?:Windows Phone)/.test(ua),
				isSymbian = /(?:SymbianOS)/.test(ua) || isWindowsPhone,
				isAndroid = /(?:Android)/.test(ua),
				isFireFox = /(?:Firefox)/.test(ua),
				isChrome = /(?:Chrome|CriOS)/.test(ua),
				isTablet = /(?:iPad|PlayBook)/.test(ua) || (isAndroid && !/(?:Mobile)/.test(ua)) || (isFireFox &&
					/(?:Tablet)/.test(ua)),
				isPhone = /(?:iPhone)/.test(ua) && !isTablet,
				isPc = !isPhone && !isAndroid && !isSymbian;
			return {
				isTablet: isTablet,
				isPhone: isPhone,
				isAndroid: isAndroid,
				isPc: isPc
			};
		}();
		if (os.isAndroid || os.isPhone) { // 手机
			devide.value = 'phone'
		} else if (os.isTablet) { // 平板
      devide.value = 'pad'
		} else if (os.isPc) { // 电脑
      devide.value = 'pc'
		}
}
const isOrientation = ref(false)
function orientationChange() {
  if (window.orientation == 180 || window.orientation == 0) { 
    isOrientation.value = true
  } 
  if (window.orientation == 90 || window.orientation == -90 ){ 
    isOrientation.value = false
  } 
};
window.addEventListener("orientationchange",orientationChange);

onMounted(()=>{
  screenWidth.value = document.body.clientWidth
  window.onresize = () => {
    return (() => {
      orientationChange()
      screenWidth.value = document.body.clientWidth
    })()
  }
})

watch(()=>screenWidth.value,(newVal,oldVal)=>{
  if(newVal){
    getOs()
    orientationChange()
    getData()
  }
},{ deep: true })
</script>
<style lang="less">
@import url(@/style/main.less);
</style>
