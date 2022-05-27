<script>


export default {
  props: {
    list_adds: { type: Object },
  },
  name: "app",
  data() {
    return {
      status: "idle",
      score_modal: false,
      game_over_modal: false, 
      game_crear_modal: false,
      time: 10,
      timer: null,
      mole_score: 0,
      mole_score_minus: 0, 
      mole_img: "src/assets/moleimg.png",
      obj_img: "",
      random_id: "",
      live_sec: 2,
      // img: document.querySelector("image")

      //list
      nic_name: "닉네임을 입력하세요.",
      num: 0, 
    }; 
  }, 

  methods: {
    timer_start() { 
      if (this.status != "idle") {
        console.log(`status is not idle`);
        return;
      }

      this.status = "running";  //게임 상태 idle인지 running인지 나타냄
      if (this.status == "running") {
        this.timer = setInterval(() => {
          if (this.time >= 1) {
            this.time = this.time - 1;
            console.log(`time:${this.time}`);
            console.log(`timer:${this.timer}`);

            //mole random
            this.live_sec -= 1; //랜덤으로 나오는 두더지 나오는 시간 -1씩 감소
            if (this.live_sec == 0) {
              let random = Math.floor(Math.random() * 9);  //실시간 시간이 0일때 랜덤으로 id값과 random값이 같을 때 이미지 출력
              let row = parseInt(random / 3); 
              let col = random % 3;
              //이미지의 id값 ran_img_${col}_${row} 형태로 가져오기
              let obj_img = document.getElementById(`ran_img_${col}_${row}`); 
              console.log(obj_img);
              obj_img.src = this.mole_img; //이미지
              this.random_id = `ran_img_${col}_${row}`; //random id value
              console.log(`이미지 랜덤: ${obj_img.src}`);
              console.log(` 랜덤: ${random}, row 값: ${row}, col 값: ${col}`);
              this.live_sec =2; //실시간 초가 0이 되면 다시 3으로 설정되어 반복
            }
          } else { 
            this.game_over_modal =! this.game_over_modal; 
            this.time = 90 - this.time;
            clearInterval(this.timer); //timer 정지
          }
        }, 1000);
      } 
    }, 

    redirect() {
      clearInterval(this.timer);
      this.status = `idle`; 
      this.time = 10;
      this.mole_score = 0;
      this.mole_score_minus =0;
      this.score_modal = false;
      this.game_over_modal = false;
      this.game_crear_modal = false; 
      this.toggle = false; 
      this.random_id = "";
      this.nic_name="닉네임을 입력하세요."; 
      this.num = this.num;
    },
 
    img_click(event) { 
      this.mole_score = this.mole_score + 1;
      this.random_id = "";  //id ="" 되면서 이미지 사라짐 및 더블클릭 방지
      console.log(`두저지 클릭 + :${this.mole_score}`);
      if (this.mole_score >= 30) {
        this.game_crear_modal = !this.game_crear_modal;
        this.time = 90 - this.time;
        clearInterval(this.timer);
      }
    },  
    td_click(e) {
      if (this.status == "idle") {
        return this.redirect(); 
      }
      this.mole_score = this.mole_score - 1;
      this.mole_score_minus = this.mole_score_minus - 1; //감점점수 카운트
      if (this.mole_score < 0) {
        this.score_modal =! this.score_modal; 
        clearInterval(this.timer);
      }
    },

    list_save() { 
      this.game_over_modal = false;
      this.game_crear_modal = false; 
       
      this.num = this.num +1;     
      console.log(`등록번호: ${this.num}`);
      let input_name = this.nic_name; 
      console.log(`닉네임: ${input_name}`);
      let moletotal = this.mole_score; 
      console.log(`잡은 두더지 수: ${moletotal}`);
      let minus = this.mole_score_minus; 
      console.log(`마이너스: ${minus}`);
      let totaltime = this.timer_value;  
      if(this.time == 90 ){ 
        totaltime = `-`;
      } 
      console.log(`걸린 시간: ${totaltime}`); 
      let score;
      if (this.mole_score >= 0 && this.mole_score < 6) {
        score = "C";
      } else if (this.mole_score >= 6 && this.mole_score < 16) {
        score = "B";
      } else if (this.mole_score >= 16 && this.mole_score < 26) {
        score = "A";
      } else if (this.mole_score >= 26 && this.mole_score < 31) {
        score = "S";
      }
      console.log(`등급: ${score}`);
 
      //list_add에 값 push app.vue에 값 들어감
      this.list_adds.push({
        num: this.num,
        input_name: input_name, 
        moletotal: moletotal,
        minus: minus, 
        totaltime: totaltime,
        score: score,
      });
      console.log(`리스트 push`, this.list_adds);
      return this.redirect();
    },
  },
 
  computed: {
    timer_value(){
      const mm = parseInt(this.time / 60);
      const ss = parseInt(this.time % 60);
      return `${mm.toString().padStart(2, "0")}:${ss
        .toString()
        .padStart(2, "0")}`;
    },
  },

  components: {},
};
</script> 



<template>
  <div class="mole play">
    <!-- <h1> 두더지 잡기 게임 </h1> -->
    <div class="timer-box">
      <h1 class="time">{{ timer_value }}</h1>
    </div> 
    <div class="live">{{ `${mole_score}마리` }}</div>

    <div>
      <table class="play-box" border="1">
        <tr v-for="col in [0, 1, 2]" :key="col"> 
          <td v-for="row in [0, 1, 2]" :key="row" @click="td_click($e)" id="moletd">
            <!-- onclick="event.cancelBubble = true;" -> td클릭 적용되지 않고 img클릭만 적용 -->
            <!-- 랜덤으로 출력된 수와 id값이 같으면 이미지 출력 -->
            <img 
              v-show="`ran_img_${col}_${row}` == random_id"
              :src="`${obj_img}`" :id="`ran_img_${col}_${row}`"
              onclick="event.cancelBubble = true;"
              @click="img_click($event)" 
              style="width: 75%; height: 75%;"
            />
          </td>
        </tr> 
      </table>
    </div> 

    <div>
      <button class="but" @click="redirect">reset</button> &nbsp;&nbsp;
      <button class="but" v-on:click="timer_start">start</button>
    </div>

    <!-- 점수 모달창 -->
    <div id="modal" v-if="score_modal == true">
      <div id="T_game">
        <h1>game</h1>
        <h1>over</h1>
      </div>
    </div>

    <!-- 시간 모달창 -->
    <div id="modal" v-if="game_over_modal == true">
      <div id="T_game">
        <h1>game</h1>
        <h1>over</h1>
        <hr />
        <input type="text" v-model="nic_name" id="mo_input" />
        <div>
          <button class="but" @click="redirect">취소</button> &nbsp;&nbsp;
          <button class="but" @click="list_save">등록</button>
        </div>
      </div>
    </div> 

    <!-- 게임 클리어 모달창 -->
    <div id="modal" v-if="game_crear_modal == true">
      <div id="T_game">
        <h1>game</h1>
        <h1>crear</h1>
        <hr /> 
        <input type="text" v-model="nic_name" id="mo_input" /> 
        <div>
          <button class="but" @click="redirect">취소</button> &nbsp;&nbsp;
          <button class="but" @click="list_save">등록</button>
        </div>
      </div>
    </div>
  </div>
</template>




 

 


<style>
.timer-box {
  width: 200px;
  height: 50px;
  border: 3px solid transparent;
  border-color: black;
  margin-left: 140px;
  margin-bottom: 10px;
  margin-top: 20px;
}
.time {
  font-family: "Press Start 2P", cursive;
  text-align: center;
}

.play-box {
  width: 480px;
  height: 480px;
  border: 3px solid transparent;
  border-color: black;
  border-spacing: 10px;
}

#moletd {
  width: 150px;
  height: 150px;
  text-align: center;
}

.but {
  display: inline-block;
  padding: 15px 25px;
  font-size: 15px;
  cursor: pointer;
  text-align: center;
  text-decoration: none;
  outline: none;
  color: black;
  background-color: #c29565;
  border: none;
  border-radius: 15px;
  box-shadow: 0 8px #999;
  margin-top: 20px;
  font-family: "Press Start 2P", cursive;
}

.but:hover {
  background-color: #3aa03d;
}

.but:active {
  background-color: #3e8e41;
  box-shadow: 0 5px #666;
  transform: translateY(4px);
}
.live {
  text-align: right;
  font-family: "Press Start 2P", cursive;
}

/* 모달창 css */

#modal {
  position: absolute;
  width: 100%;
  height: 75%;
  background: rgba(0, 0, 0, 0.8);
  top: 0;
  left: 0;
  /* display: none; */
}

#T_game {
  position: absolute;
  width: 90%;
  height: 70%;
  background: white;
  top: 65px;
  left: 30px;
  font-family: "Press Start 2P", cursive;
  font-size: 200%;
  text-align: center;
}

#mo_input {
  font-family: "Press Start 2P", cursive;
  width: 35%;
  height: 10%; 
  background: rgb(195, 187, 187);
}

/* list css */
.f {
  font-family: "Press Start 2P", cursive;
  text-align: center;
}
.list_table {
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