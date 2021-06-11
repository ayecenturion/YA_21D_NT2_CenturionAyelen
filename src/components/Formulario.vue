<template >

  <section class="src-components-formulario">
    <div class="jumbotron">
    <h4><u>Ingrese los datos del alumno</u></h4>
    <br>
    <vue-form :state="FormState" @submit.prevent="enviar()">
      <validate tag="div">
        <label for="nombreAlumno">Nombre del alumno</label>
        <input type="text" id="nombreAlumno" name="nombreAlumno" class="form-control" autocomplete="off" v-model.trim="FormData.nombreAlumno" required @input= "FormDirty.nombreAlumno=true" :minlength="nombreAlumnoLongMin" :maxlength="nombreAlumnoLongMax">
        
        <field-messages name="nombreAlumno" show="$dirty">
          <div slot="required" class="alert alert-danger mt-1">Campo requerido</div>
          <div slot="no-espacios" class="alert alert-danger mt-1">No se permiten espacios intermedios en este campo</div>
          <div slot="minlength" class="alert alert-danger mt-1">
            El campo requiere como minimo {{nombreAlumnoLongMin}} caracteres</div>
          <div v-if="FormData.nombreAlumno.length == nombreAlumnoLongMax" class="alert alert-danger mt-1">
            El campo requiere como máximo {{nombreAlumnoLongMax}} caracteres</div>
        </field-messages>
      </validate>
      <br>
      <validate tag="div">
        <label for="apellidoAlumno">Apellido del alumno</label>
        <input type="text" id="apellidoAlumno" name="apellidoAlumno" class="form-control" autocomplete="off" v-model.trim="FormData.apellidoAlumno" required @input= "FormDirty.apellidoAlumno=true" :minlength="apellidoAlumnoLongMin" :maxlength="apellidoAlumnoLongMax">
        
        <field-messages name="apellidoAlumno" show="$dirty">
          <div slot="required" class="alert alert-danger mt-1">Campo requerido</div>
          <div slot="no-espacios" class="alert alert-danger mt-1">No se permiten espacios intermedios en este campo</div>
          <div slot="minlength" class="alert alert-danger mt-1">
            El campo requiere como minimo {{apellidoAlumnoLongMin}} caracteres</div>
          <div v-if="FormData.apellidoAlumno.length == apellidoAlumnoLongMax" class="alert alert-danger mt-1">
            El campo requiere como máximo {{apellidoAlumnoLongMax}} caracteres</div>
        </field-messages>
      </validate>
      <br>
      <validate tag="div">
        <label for="nota">Nota</label>
        <input type="number" id="nota" name="nota" class="form-control" autocomplete="off" v-model.number="FormData.nota" required :min="notaMin" :max="notaMax">

        <field-messages name="nota" show="$dirty">
          <div slot="required" class="alert alert-danger mt-1">Campo requerido</div>
          <div slot="min" class="alert alert-danger mt-1">Tiene que ingresar una nota mayor a {{notaMin}} </div>
          <div slot="max" class="alert alert-danger mt-1">Tiene que ingresar una nota menor a {{notaMax}} </div>
        </field-messages>
      </validate>
      <br>
      <button class="btn btn-success my-3" :disabled="FormState.$invalid" type="submit">Enviar</button>
    </vue-form>
  </div>
  <div class="table-responsive">
    <hr>
    <br>
    <p v-if="FormData.nombre != FormState.$dirty"><u>Datos ingresados: </u></p>
    <br>

    <!-- <table class="table" v-if="FormData.nombreAlumno != FormState.$dirty">
      <thead>
        <tr class="text-gray" style="background-color: #84ffd4;">
          <th scope="col">Nombre Completo</th>
          <th scope="col">Nota</th>
        </tr>
      </thead>
      <tbody>
        <tr class="text-gray" :style="getEstiloLetra()">
          <td>{{FormData.nombreAlumno}} {{FormData.apellidoAlumno}}</td>
          <td>{{FormData.nota}}</td>
        </tr>
        <tr class="text-gray" :style="getEstiloLetra()">
          <th v-if="this.cantNotas != 0 ">Promedio Total {{this.totalNotas/ this.cantNotas}}</th>
        </tr>
      </tbody>
    </table> -->
      <div v-if="notas.length" class="table-responsive" >
        <table class="table table-bordered" >
          <tr>
            <th v-for="(col,index) in getCols" :key="index">{{col}}</th>
          </tr>
          <tr v-for="(nota,index) in notas" :key="index">
            <td v-for="(col,index) in getCols" :key="index">{{nota[col]}}</td>
          </tr>
          <tr class="text-gray">
            <th v-if="this.cantNotas != 0 ">Promedio Total</th>
            <th :style="getEstiloLetra()">{{this.totalNotas/ this.cantNotas}}</th>
          </tr>
        </table>
      </div>
   

    <p v-else class="alert alert-warning">
      Todavía no ha ingresado datos
    </p>

  </div>
  </section>

</template>

<script lang="js">

  export default  {
    name: 'src-components-formulario',
    props: [],
    mounted () {

    },
    data () {
      return {
        FormData:this.getInicialData(),
        FormState:{},
        nombreAlumnoLongMin : 3,
        nombreAlumnoLongMax : 15,
        apellidoAlumnoLongMin : 3,
        apellidoAlumnoLongMax : 15,
        notaMin : 0,
        notaMax : 10,
        notas : [],
        totalNotas : 0,
        cantNotas: 0
      }
    },
    methods: {
      getInicialData(){
        return{
          nombreAlumno: '',
          apellidoAlumno:'',
          nota:''
        }
      },
      enviar(){
      var nota = {
        nombreCompleto: `${this.FormData.nombreAlumno} ${this.FormData.apellidoAlumno}`,
        nota: this.FormData.nota,
      }
      this.totalNotas += this.FormData.nota
      this.cantNotas++

      this.notas.push(nota)
      console.log({...this.FormData})
      this.FormData = this.getInicialData()
      this.FormState._reset()
      },
      getEstiloLetra(){
      if(this.totalNotas <=3){
        return{
          'color': 'red'
        }
      }else if(this.totalNotas > 3 && this.totalNotas <= 6){
        return{
          'color': 'yellow'
        }
      }else if(this.totalNotas>= 7){
        return{
          'color': 'green'
        }
      }
    },
    },
  computed: {
    getCols(){
        return Object.keys(this.notas[0])
      }
  }
}


</script>

<style scoped lang="css">
  .src-components-formulario {

  }
h4 {
    color:black;
}
.jumbotron{
    padding: 32px !important;
}
</style>
