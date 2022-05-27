<script>
export default {
  name: "app",
  props: {
    list_adds: { type: Object },
  },

  data() {},

  methods: {},
};
</script> 

<template>
  <!-- list -->
  <div class="form">
    <div>
      <h1 class="list_form">list</h1>
    </div>
    <div>
      <table class="list_table" border="1">
        <div v-for="record in list_adds" v-bind:key="record">
        <tr>
          <td>번호</td>
          <td>닉네임</td>
          <td>잡은 두더지 수</td>
          <td>감점</td>
          <td>걸린 시간</td>
          <td>등급</td> 
        </tr> 
         <tr> 
          <td>{{ record.num}}</td>
          <td>{{ record.input_name }}</td>
          <td>{{ record.moletotal }}</td>
          <td>{{ record.minus }}</td>
          <td>{{ record.totaltime }}</td>
          <td>{{ record.score }}</td>
        </tr>
        </div> 
      </table>
    </div>
  </div>
</template>
    

<style>
.list_form { 
  font-family: "Press Start 2P", cursive;
  text-align: center;
}
.list {
  font-family: "Press Start 2P", cursive;
  text-align: center;
  margin-left: 55px;
  border: 3px solid black;
  border-collapse: collapse;
}

#ttt {
  border: 2px solid black;
  border-collapse: collapse;
}

.form {
  margin-top: 180px;
  margin-left: 10px;
  align-items: center;
  justify-content: center;
}
</style>

<style>
@import url("https://fonts.googleapis.com/css2?family=Press+Start+2P&display=swap");
</style>
