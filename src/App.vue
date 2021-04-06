<template>
  <div id="app">
    <div class="gif"></div>
    <h2>Cuenta atrás</h2>
    <div class="cuadro my-3"> {{tiempo}} </div>
    <div>
      <button v-for="btn in data" :key="btn.name" @click="botones(btn.value)" class="btn btn-outline-primary m-3">{{ btn.name }}</button>
    </div>
    <button @click="start" class="btn btn-success m-4" type="button">Comenzar</button>
    <button @click="stop" class="btn btn-danger m-4" type="button">Detener</button>
    <hr>
<!--Cronometro 2-->
    <div class="gif"></div>
    <h2>Cronómetro</h2>
    <div>
      <input v-model="hor" type="number" class="inputtime mx-3 mt-3 text-center" placeholder="time">
      <input v-model="min" type="number" class="inputtime mx-3 mt-3 text-center" placeholder="time">
      <input v-model="seg" type="number" class="inputtime mx-3 mt-3 text-center" placeholder="time">
    </div>
    <div>
      <div class="reloj my-4 mx-2"> {{hor}} </div>
      <div class="dosp my-4 mx-2"> : </div>
      <div class="reloj my-4 mx-2"> {{min}} </div>
      <div class="dosp my-4 mx-2"> : </div>
      <div class="reloj my-4 mx-2"> {{seg}} </div>
    </div>
    <button @click="start2" class="btn btn-success m-3" type="button">Comenzar</button>
    <button @click="stop2" class="btn btn-primary m-3" type="button">Detener</button>
    <button @click="restart" class="btn btn-danger m-3">Reiniciar</button>
    <h2 class="fw-bold">{{ final }}</h2>
  </div>
</template>


<!--Javascript-->
<script>
export default {
  name: "App",
  data() {
    return {
      tiempo: 10,
      intervalo: "",
      data: [
        {
          name: "3s",
          value: 3
        },
        {
          name: "1m",
          value: 60
        },
        {
          name: "5m",
          value: 300
        },
        {
          name: "10m",
          value: 600
        },
        {
          name: "30m",
          value: 1800
        },
      ], 
      // Cronometro 2
      intervalo2: "",
      min: 0,
      seg: 0,
      hor: 0,
      final: ""
    }
  },

  watch: {
    tiempo(v) {
      console.log(v);
      if(v===0) this.stop()
    },
    // CRonometro 2
    seg(v) {
      console.log(v);
      if(v===0) {
        if(this.hor===0 && this.min===0) {
          setTimeout(()=> {
          this.restart()
          console.log("SE ACABO EL TIEMPO");
          this.final = "SE ACABO EL TIEMPO"
          },1000)
        } else {
          this.min--
          setTimeout(() =>{
            this.seg = 59
          },1000)
        }
      }
      if(v > 60) {
        this.seg = 60
      }
    },
    min(v) {
      if(v===0) {
        this.hor--
        setTimeout(() =>{
          this.min = 59
        },1000)
      }
      if(v===-1) {
        this.hor--
        this.min = 59
      }
      if(v > 60) {
        this.min = 60
      }
    },
    hor(v) {
      if(v===-1) {
        this.hor = 0
      }
      if(v > 60) {
        this.hor = 60
      }
    },
  },

  methods: {
    start() {
      this.intervalo = setInterval(() => {
        this.tiempo--
      }, 1000)   
    },
    stop() {
      clearInterval(this.intervalo);
    },
    botones(v) {
      console.log(v);
      this.tiempo = v
    },
    // Cronometro 2
    start2() {
      this.intervalo2 = setInterval(() => {
        this.seg--
        this.final = ""
      }, 1000)   
    },
    stop2() {
      clearInterval(this.intervalo2);
    },
    restart() {
      this.hor = 59
      this.min = 59
      this.seg = 59
      clearInterval(this.intervalo2)
    },

 
  }
}
</script>


<!--CSS-->
<style lang="scss">
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}
.cuadro {
  margin: 0 auto;
  width: 150px;
  height: 80px;
  font-family: 'VT323', monospace;
  font-size: 50px;
  border: 2px solid green;
  background-color: rgb(217, 255, 217);
  border-radius: 10px;
}
.gif {
  margin: 0 auto;
  width: 370px;
  height: 230px;
  background-image: url("./assets/time.gif");
  background-size: cover;
}
// Cronometro 2
.inputtime {
  width: 70px;
}
.reloj {
  display: inline-block;
  background-color: rgb(180, 180, 180);
  font-family: 'VT323', monospace;
  font-size: 40px;
  width: 90px;
}
.dosp {
  display: inline-block;
  font-family: 'VT323', monospace;
  font-size: 40px;
}
</style>
