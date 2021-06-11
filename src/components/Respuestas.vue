<template>

  <section class="src-components-respuestas">
    <div v-if="preguntas.length" class="table-responsive">
        <table class="table table-bordered">
          <tr>
            <th v-for="(col,index) in getCols" :key="index">{{col}}</th>
          </tr>
          <tr v-for="(pregunta,index) in preguntas" :key="index">
            <td v-for="(col,index) in getCols" :key="index">{{pregunta[col]}}</td>
          </tr>
        </table>
    </div>    
  </section>

</template>

<script lang="js">

  export default  {
    name: 'src-components-respuestas',
    props: [],
    mounted () {

    },
    data () {
      return {
        preguntas : [
          {numero: 1, respuesta: 'c'},
          {numero: 2, respuesta: 'c'},
          {numero: 3, respuesta: 'b'}
        ]
      }
    },
    methods: {

    },
    computed: {
      getCols(){
        return Object.keys(this.preguntas[0])
      }
    }
}


</script>

<style scoped lang="css">
  .src-components-respuestas {

  }
</style>
