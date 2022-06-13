<script>
export default {
  name: "app",

  props: {
    list_adds: { type: Object },
  },

  setup() {
    return {
      columns,
    };      
  }, 
   
};

const columns = [
  {
    name: "q_num", 
    //visible-columns true면 항상 col 표시
    required: true,
    label: "번호",
    align: "left",
    // row object 속성은 field 속성에 매핑되는 열 또는 함수의 값 결정
    field: list_adds => list_adds.num, 
    sortable: true,
  },
  {
    name: "q_input_name",
    align: "center",
    label: "닉네임",
    field: "input_name",    
    sortable: true,  
  }, 
  {      
    name: "q_moletotal",
    label: "잡은 두더지 수",      
    field: "moletotal",  
    sortable: true,              
  },  
  { name: "q_minus", label: "감점 점수", field: "minus" },    
  { name: "q_totaltime", label: "걸린 시간", field: "totaltime" },
  { name: "q_score", label: "등급", field: "score" },     
];  
</script>            
             
                                                 
                                
<template>     
  <div class="q-pa-md"> 
    <q-table title="List" :rows="list_adds" :columns="columns" row-key="q-num" >  
    <q-td></q-td>  
    </q-table>  
  </div>  
</template>     
 
  
                   
<style>      
</style>    
   
   