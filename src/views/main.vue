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
              <span>{{info}}</span>
              <span></span>
            </div>
          </a>
      </div>
    </div>
    <div class="footer">{{page.footer}}</div>
  </div>
</template>
<script lang="ts" setup>
import axios from "axios";
import { onMounted, ref, reactive, watchEffect, watch } from "vue";
const page = reactive<any>({
  title:'',
  list:[],
  footer:''
})
const info = ref('Visit my page')
const screenWidth = ref<any>()
const getData = () => {
  try{
      axios.get('./static/json/main.json').then(response=>{
          const {data} = response
          page.list = []
          page.desc = data.desc
          page.title = data.title
          page.footer = data.footer
          if(screenWidth.value < 700) {
            page.list.push(data.father_img)
            data.mother_img && page.list.push(data.mother_img)
            data.daughter_img && page.list.push(data.daughter_img)
          } else {
            data.mother_img && page.list.push(data.mother_img)
            data.father_img && page.list.push(data.father_img)
            data.daughter_img && page.list.push(data.daughter_img)
          }
      })
    }catch{}
}

onMounted(()=>{
  screenWidth.value = document.body.clientWidth
  window.onresize = () => {
    return (() => {
      screenWidth.value = document.body.clientWidth
    })()
  }
})
watch(()=>screenWidth.value,(newVal,oldVal)=>{
  if(newVal){
    getData()
  }
},{ deep: true })
</script>
<style lang="less">
@import url(@/style/main.less);
</style>
