<template>
  <div class="main-app">
    <div class="top-progress">
      <strong style="margin-right: 10px">Lv {{ lv }}</strong>
      <Progress :percentage="point"/>
    </div>
    <div class="main-content">
      <div class="weather">
        <div>
          <div v-if="drop" class="raindrop raindrop3"></div>
          <div v-if="drop" class="raindrop raindrop3"></div>
          <div v-if="drop" class="raindrop raindrop3"></div>
        </div>
      </div>

      <div class="flow-wrap">
        <img class="tree" alt="#" :src="treeSrc"/>

      </div>

    </div>
    <div class="footer">
      <Fab></Fab>

      <div v-if="astimate" class="footer-content">
        <span v-if="drop" class="increment">+{{ incrementPoint }}</span>
        <img v-if="!wattering" id="btn-cursor" alt="#" src="@/assets/icons/cursor.png"/>
        <div style="float: right;; width: 40px">
          <img
              @click="onWattering"
              :class="wattering ? 'btn-can-active' : 'btn-can'"
              alt="#"
              src="@/assets/icons/can.png"
          />
        </div>


      </div>
      <div v-else class="footer-counter">
        <vue-countdown-timer

            @end_callback="endCallBack('event ended')"
            :start-time="startTime"
            :end-time="endTime"
            :interval="1000"
            label-position="begin"
            :day-txt="'d'"
            :hour-txt="'h'"
            :minutes-txt="'m'"
            :seconds-txt="'s'">
        </vue-countdown-timer>
      </div>
    </div>
    <Modal @close="dialogVisible = false" :modal-visible="dialogVisible">
      <span slot="header" style="text-align: center; display: block"
      >Cây của bạn đã lên lv
        {{ lv }}</span
      >
      <div slot="body">
        <img class="tree" alt="#" :src="treeSrc"/>
      </div>
      <div slot="footer">
        <button class="modal-default-button btn-2" @click="dialogVisible = false">OK</button>
      </div>
    </Modal>
  </div>
</template>

<script>
import Progress from '@/components/Progress'
import {Images} from "@/assets/Images";
import Fab from "@/components/button/Fab";
export default {
  components: {
    Progress,
    Fab
  },
  data() {
    return {
      wattering: false,
      point: 0,
      dialogVisible: false,
      drop: false,
      lv: 1,
      astimate: false,
      startTime: 0,
      endTime: 0,
      incrementPoint: 10
    };
  },
  computed: {
    treeSrc() {
      switch (this.lv) {
        case 1:
          return Images.p1
        case 2:
          return Images.p2
        case 3:
          return Images.p3
        case 4:
          return Images.p4
        case 5:
          return Images.p5
        case 6:
          return Images.p6
        case 7:
          return Images.p7
        case 8:
          return Images.p8
        case 9:
          return Images.p9
        case 10:
          return Images.p10
        case 11:
          return Images.p11
        case 12:
          return Images.p12
      }
    }
  },
  watch: {
    point() {
      if (this.point === 100) {
        this.lv += 1;
        this.point = 0
      }
    }
  },
  methods: {
    onWattering() {
      this.wattering = true;
      this.point += this.incrementPoint;
      if (this.point === 100) {
        this.dialogVisible = true;
      }

      setTimeout(() => {
        this.drop = true;
      }, 1500);
      setTimeout(() => {
        this.drop = false;
      }, 3000);
      setTimeout(() => {
        this.wattering = false;
        this.startTime = new Date(Date.now())
        this.endTime = new Date(this.startTime.setHours(this.startTime.getHours() + 5))
        this.astimate = false
      }, 5000);
    },

    endCallBack: function (x) {
      this.astimate = true
      console.log(x);
    },
  },
};
</script>

<style lang="scss">
@use "src/css";

.top-progress {
  display: flex;
  flex-direction: row;
  justify-content: center;
  align-items: center;
  margin-top: 20px;
}

.main-app {
  display: flex;
  flex-direction: column;
  justify-content: space-between;
  align-items: center;

  .main-content {
    display: block;

    .weather {
      display: block;
      position: relative;
      transform: rotate(35deg);
      z-index: 10;

      .raindrop {
        width: 5px;
        height: 5px;
        float: right;
        border-radius: 80% 0 55% 50% / 55% 0 80% 50%;
        border: 3px solid css.$raindrop;
        background: css.$raindrop;
        display: inline-block;
        @include css.rotate(-25deg);
        position: relative;
        backface-visibility: hidden;
      }

      .raindrop3 {
        animation: raindrop 2s infinite linear, raindropFade 2s linear;
      }
    }

    .flow-wrap {
      position: relative;
      margin-top: 55%;
    }

  }
}
.footer-content {
  width: 150px;
  transition: .4s ease-in-out;
  margin-bottom: 1rem;

  #btn-cursor {
    width: 40px;
    position: relative;
    animation: mymove 1s infinite;
    animation-timing-function: linear;
    float: left;
  }

  .btn-can {
    width: 40px;
    cursor: pointer;
    position: relative;
  }

  .btn-can-active {
    width: 40px;
    cursor: pointer;
    position: relative;
    animation: bt_to_top 5s;
  }
}
.footer-counter {
  border: 2px solid;
  padding: 5px;
  border-radius: 10px;
  position: relative;
  transition: .4s linear;
  text-align: center;
  color: #FFFFFF;
  font-size: 14px;
  font-weight: 600;
  background-color: #C5834C;
}
.footer {
  display: flex;
  justify-content: space-between;
  align-items: center;
  width: 100%;
  height: 100px;
}
.increment {
  position: relative;
  animation: plus 3s;
}

@keyframes plus {
  0% {
    bottom: 0;
    opacity: 0.4%;
  }
  50% {
    bottom: 400px;
    opacity: 1;
  }
  70% {
    opacity: 0.3%;
  }

  100% {
    opacity: 0.2%;
  }

}

.tree {
  object-fit: cover;
  width: 300px;
  height: 300px;
  z-index: 1;
}

@keyframes mymove {
  from {
    left: 0;
  }
  to {
    left: 50px;
  }
}

@keyframes raindrop {
  from {
    top: 3rem;

  }
  to {
    top: 15rem;

  }
}

@keyframes raindropAlt {
  from {
    top: 3rem;
  }
  to {
    top: 12rem;
  }
}

@keyframes raindropFade {
  75% {
    opacity: 1;
  }
  100% {
    opacity: 0;
  }
}

@keyframes bt_to_top {
  0% {
    bottom: 0;
  }
  50% {
    bottom: 28rem;
    transform: rotate(-100deg);
  }

  70% {
    bottom: 12.5rem;
  }
  100% {
    bottom: 0;
  }
}
</style>
