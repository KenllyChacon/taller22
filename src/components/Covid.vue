<template>

  <h1>CASOS COVID</h1>

  <div>
    <ul>
      <li v-for="state in arreglo">
        <span>{{ state }}</span>
      </li>
    </ul>

    <input v-model="text" v-on:keypress.enter="buscar()" type="text" placeholder="Ingrese codigo de estado">
    <!--    <h1 v-show="false">{{this.consumirAPIObtenerIniciales()}}</h1>-->
    <div id="resultadoBusqueda" v-if="resultadoBusqueda">

      <div class="salida">
        <label>Casos Positivos:</label>
        <input type="text" :value="this.casosPositivos">
      </div>

      <div class="salida">
        <label>Total Resultados Pruebas:</label>
        <input type="text" :value="this.totalResultadosPruebas">
      </div>

      <div class="salida">
        <label>Actualmente Hospitalizados:</label>
        <input type="text" :value="this.actualmenteHospitalizados">
      </div>

      <div class="salida">
        <label>Muertos: </label>
        <input type="text" :value="this.muertos">
      </div>

      <div class="salida">
        <label>Casos Positivos Virales:</label>
        <input type="text" :value="this.casosPositivosVirales">
      </div>

      <div class="salida">
        <label>Numero Aumento Muertes:</label>
        <input type="text" :value="this.numeroAumentoMuertes">
      </div>
    </div>
  </div>

</template>

<script>
export default {
  name: "Covid",
  data() {
    // this.consumirAPIObtenerIniciales()
    return {
      // arreglo: this.consumirAPIObtenerIniciales(),
      arreglo:null,
      text: null,

      casosPositivos: null,
      totalResultadosPruebas: null,
      actualmenteHospitalizados: null,
      muertos: null,
      casosPositivosVirales: null,
      numeroAumentoMuertes: null,

      resultadoBusqueda: false
    }
  },
  methods: {
    async consumirAPI(id) {
      const data = await fetch("https://api.covidtracking.com/v1/states/" + id + "/current.json").then(r => r.json())
      const {positive, totalTestResults, hospitalizedCurrently, death, totalTestsViral, deathIncrease} = data
      this.casosPositivos = positive
      this.totalResultadosPruebas = totalTestResults
      this.actualmenteHospitalizados = hospitalizedCurrently
      this.muertos = death
      this.casosPositivosVirales = totalTestsViral
      this.numeroAumentoMuertes = deathIncrease
    },
    async consumirAPIObtenerIniciales() {
      const arreglos = []
      const date = await fetch("https://api.covidtracking.com/v1/states/current.json").then(r => r.json())
      for (const it in date) {
        const valor = date[it]
        const {state} = valor
        arreglos.unshift(state)
        this.arreglo = arreglos
      }
    },
    async buscar() {
      for (const valor of this.arreglo) {
        if (valor === this.text) {
          await this.consumirAPI(valor.toLowerCase())
        }

      }
      this.resultadoBusqueda = true
    }
  },
  // Se ejecuta apenas inice la pagina
  mounted() {
    console.log("mounted: Mi componente se monto")
    this.consumirAPIObtenerIniciales()
  },
  beforeCreate() {//antes que se cree el
    console.log("beforeCreate: Antes de crear el componente")
  },
  created() {
    console.log("Created")
  },
  beforeMount() {//antes de que se monte
    console.log("beforeMount")
  },
  beforeUpdate() {
    console.log("beforeUpdate: antes de que se actualize el componente")
  },
  updated() {
    console.log("updated: cuando el componente se actualize")
  },
  activated() {
    console.log("activated. cuando el componente esta activo")
  },
  deactivated() {
    console.log("deactivated: Decido desactivarlo")
  },
  unmount() {
    console.log("unmount")
  },
  errorCaptured(){
    console.log("errorCaptured")
  },
  renderTracked(){
    console.log("renderTracked")
  },
  renderTriggered(){
    console.log("renderTriggered")
  }
}
</script>

<style scoped>

ul li {
  margin: 10px;
}

ul {
  column-count: 4;
  margin: 25px;
  font-size: 20px;
}

#resultadoBusqueda  {
  width: 310px;
  margin: 15px auto;
}

input {
  left: 15px;
  font-size: 20px;
  color: white;
  width: 225px;
  text-align: center;
}

.salida label{
  text-align: left;
}

.salida{
  display: grid;
  margin: 25px;
  font-size: 20px;
}

</style>