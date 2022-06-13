<template>
  <q-layout view="lHh Lpr lFf">
    <q-header elevated class="glossy">
      <q-toolbar>
 

        <q-toolbar-title>
          Quasar App
        </q-toolbar-title>

        <div>Quasar v{{ $q.version }}</div>
      </q-toolbar>
    </q-header>

 

    <q-page-container>
      <div class="q-pa-md"> 
        <div class="row">
          <div class="col-6">
            <Mole :list_adds="record_list" />
          </div>
          <div class="col-6">
            <list :list_adds="record_list" />
          </div> 
        </div>
      </div>
             
    </q-page-container> 
  </q-layout>   
</template>  
  
<script>
import Mole from './components/Mole.vue'
import list from './components/list.vue'
import { ref } from 'vue' //vue에서 ref import 해옴
 
export default {
  name: 'LayoutDefault',
    
  components: {  
    Mole,
    list,
  },   
 
  setup () { 
    //setup() 안에서 데이터 생성/ ref(데이터) 함수 사용
    //모든 데이터를 refrence data type로 감싸야 실시간으로 반영
    //return 하면 template에서 사용 가능
    const record_list = ref([]) 
    return { record_list }  
  }  
}
</script> 