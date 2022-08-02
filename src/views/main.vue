<template>
  <div class="main">
    <div class="title">{{page.title}}</div>
    <div class="content">
      <div v-for="(item,index) in page.list" :key="index">
          <div class="ctn_img"  :style="{'background-image':'url('+item.img_src+')'}" ></div>
          <div class="ctn_name">{{item.name}}</div>
          <a class="ctn_action"  :href="getUrl(item)">
            <div :style="{backgroundColor:getColor(item)}">
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
import { onMounted, ref, reactive } from "vue";
const page = reactive<any>({
  title:'',
  list:[],
  footer:''
})
const info = ref('Visit my page')
const getData = () => {
  try{
      axios.get('./static/json/main.json').then(response=>{
          const {data} = response
          page.title = data.title
          page.footer = data.footer
          page.list = []
          page.list.push(data.mother_img)
          page.list.push(data.father_img)
          page.list.push(data.daughter_img)
      })
    }catch{
    }
}

const getColor = (data:any)=>{
  const {name} = data
  if(name==='Elena') return 'rgba(0, 167, 229, 1)'
  if(name==='Cris') return 'rgba(8, 75, 110, 1)'
  if(name==='Andreea') return 'rgba(253, 194, 16, 1)'
}
const getUrl = (data:any) => {
  const {name} = data
  if(name==='Elena') return 'elena/elena.html'
  if(name==='Cris') return 'cristianscalude/about/'
  if(name==='Andreea') return 'andreea'
}

onMounted(()=>{
  getData()
})
</script>
<style lang="less">
@import url(@/style/main.less);
</style>
