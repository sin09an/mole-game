<script>
export default {
  props: {
    // 모든 속성 전달
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

      this.status = "running"; //게임 상태 idle인지 running인지 나타냄
      if (this.status == "running") {
        this.timer = setInterval(() => {
          if (this.time >= 1) {
            this.time = this.time - 1;
            console.log(`time:${this.time}`);
            console.log(`timer:${this.timer}`);

            //mole random
            this.live_sec -= 1; //랜덤으로 나오는 두더지 나오는 시간 -1씩 감소
            if (this.live_sec == 0) {
              let random = Math.floor(Math.random() * 9); //live_sec가 0일 때 랜덤 동작
              let row = parseInt(random / 3); //table와 random id값을 얻기 위해 랜덤으로 나온 수에 /,% 연산 수행
              let col = random % 3;
              //이미지의 id값 ran_img_${col}_${row} 형태로 가져오기
              let obj_img = document.getElementById(`ran_img_${col}_${row}`);
              console.log(obj_img);
              obj_img.src = this.mole_img; //이미지
              this.random_id = `ran_img_${col}_${row}`; //random id value
              console.log(`이미지 랜덤: ${obj_img.src}`);
              console.log(` 랜덤: ${random}, row 값: ${row}, col 값: ${col}`);
              this.live_sec = 2; //실시간 초가 0이 되면 다시 2으로 설정되어 반복
            } 
          } else {
            this.game_over_modal = !this.game_over_modal;
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
      this.mole_score_minus = 0;
      this.score_modal = false; 
      this.game_over_modal = false;
      this.game_crear_modal = false;
      this.toggle = false;
      this.random_id = "";
      this.nic_name = "닉네임을 입력하세요.";
    },

    img_click(event) {
      this.mole_score = this.mole_score + 1;
      this.random_id = ""; //id ="" 되면서 이미지 사라짐 및 더블클릭 방지
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
        this.score_modal = !this.score_modal;
        clearInterval(this.timer);
      }
    },

    list_save() {
      this.game_over_modal = false;
      this.game_crear_modal = false;

      this.num = this.num + 1;
      let input_name = this.nic_name;
      let moletotal = this.mole_score;
      let minus = this.mole_score_minus;
      let totaltime = this.timer_value;
      if (this.time == 90) {
        totaltime = `-`;
      }
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

      //list_adds에 값 push
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
    timer_value() {
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
  <div class="q-pa-md">
    <div class="row justify-center">
      <div class="col-md-6 q-mb-lg">
        <q-field outlined> 
          <template v-slot:prepend>
            <q-icon name="alarm" />
          </template>

          <template v-slot:control>
            <div class="self-center full-width no-outline clock" tabindex="0">
              {{ timer_value }}
            </div>
            <div class="self-end full-width no-outline mole_su" tabindex="0">
              {{ `${mole_score}마리` }}
            </div>
          </template>
        </q-field>
      </div>
    </div>

    <div>
      <table
        class="play-box"
        border="1"
        style="margin-left: auto; margin-right: auto"
      >
        <tr v-for="col in [0, 1, 2]" :key="col">
          <td
            v-for="row in [0, 1, 2]"
            :key="row"
            @click="td_click($e)"
            id="mole_td"
          >
            <!-- onclick="event.cancelBubble = true;" -> td클릭 적용되지 않고 img클릭만 적용 -->
            <!-- 랜덤으로 출력된 수와 id값이 같으면 이미지 출력 -->
            <img
              v-show="`ran_img_${col}_${row}` == random_id"
              :src="`${obj_img}`"
              :id="`ran_img_${col}_${row}`"
              onclick="event.cancelBubble = true;"
              @click="img_click($event)"
              style="width: 75%; height: 75%"
            />
          </td>
        </tr>
      </table>
    </div>

    <div class="q-pa-md">
      <div class="row justify-center">
        <q-btn
          class="col-md-auto q-mx-xl q-mt-lg"
          rounded
          color="green-13"
          label="reset"
          @click="redirect"
        />
        <q-btn
          class="col-md-auto q-mx-xl q-mt-lg"
          rounded
          color="green-13"
          label="start"
          @click="timer_start"
        />
      </div>
    </div>

    <!-- 점수 모달창 -->
    <q-dialog v-model="score_modal">
      <q-card>
        <q-card-section>
          <div class="text-h6">Game Over</div>
        </q-card-section>

        <q-card-section class="q-pt-none">
          Reset 버튼을 눌러 초기화면으로 돌아가세요!
        </q-card-section>

        <q-card-actions align="right">
          <q-btn flat label="reset" color="primary" @click="redirect" />
        </q-card-actions>
      </q-card> 
    </q-dialog>

    <!-- 시간 모달창 -->
    <q-dialog v-model="game_over_modal" persistent>
      <q-card style="min-width: 350px">
        <q-card-section>
          <div class="text-h6">Game Over</div>
        </q-card-section>

        <q-card-section class="q-pt-none">
          <q-input dense v-model="nic_name" autofocus />
        </q-card-section>

        <q-card-actions align="right" class="text-primary">
          <q-btn flat label="Cancel" @click="redirect" />
          <q-btn flat label="Game Save" @click="list_save" />
        </q-card-actions>
      </q-card>
    </q-dialog>

    <!-- 게임 클리어 모달창 -->
    <q-dialog v-model="game_crear_modal" persistent>
      <q-card style="min-width: 350px">
        <q-card-section> 
          <div class="text-h6">Game Crear</div>
        </q-card-section>

        <q-card-section class="q-pt-none">
          <q-input dense v-model="nic_name" autofocus />
        </q-card-section>

        <q-card-actions align="right" class="text-primary">
          <q-btn flat label="Cancel" @click="redirect" />
          <q-btn flat label="Game Save" @click="list_save" />
        </q-card-actions>
      </q-card>
    </q-dialog>
  </div>
</template>


<style>
.play-box {
  width: 480px;
  height: 480px;
  border: 3px solid transparent;
  border-color: black;
  border-spacing: 10px;
}

#mole_td {
  width: 150px;
  height: 150px;
  text-align: center;
}

/* list css */
.clock {
  font-family: "Press Start 2P", cursive;
  text-align: center;
}

.mole_su {
  font-family: "Press Start 2P", cursive;
  text-align: right;
}
</style>

<style>
@import url("https://fonts.googleapis.com/css2?family=Press+Start+2P&display=swap");
</style>