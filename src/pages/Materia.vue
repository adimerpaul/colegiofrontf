<template>
<q-page>
  <div class="row">
    <div class="col-12">
      <q-form @submit.prevent="crear">
        <div class="row q-pa-xs">
          <div class="col-4">
            <q-input outlined dense label="nombre" required v-model="materia.nombre"/>
          </div>
          <div class="col-4">
            <q-input outlined dense label="codigo" required v-model="materia.codigo"/>
          </div>
          <div class="col-4 flex flex-center">
            <q-btn type="submit" outlined dense icon="send" :color="boolcrear?'positive':'yellow'" :label="boolcrear?'Insertar':'Modificar'"/>
          </div>
        </div>
      </q-form>
    </div>
    <div class="col-12">
      <q-table title="Mis cursos" :columns="columns" :rows="materias" dense>
        <template v-slot:body="props">
          <q-tr :props="props">
            <q-td key="id" :props="props">
              <q-badge>{{props.row.id}}</q-badge>
            </q-td>
            <q-td key="nombre" :props="props">
              <q-badge color="teal" >{{props.row.nombre}}</q-badge>
            </q-td>
            <q-td key="codigo" :props="props">
              <q-badge color="negative">{{props.row.codigo}}</q-badge>
            </q-td>
            <q-td key="opciones" :props="props">
              <q-btn @click="eliminar(props.row)" size="xs" icon="delete" color="negative"  label="Eliminar"/>
              <q-btn @click="modificar(props.row)" size="xs" icon="edit" color="primary"  label="Modifica"/>
            </q-td>
          </q-tr>
        </template>
      </q-table>
    </div>
  </div>
</q-page>
</template>
<script>
export default {
  data(){
    return {
      materias:[],
      materia:{nombre:''},
      boolcrear:true,
      columns:[
        {name:'id',label:'#',field:'id'},
        {name:'nombre',label:'Nombre',field:'nombre'},
        {name:'codigo',label:'Codigo',field:'codigo'},
        {name:'opciones',label:'Opciones',field:'opciones'},
      ]
    }
  },
  created() {
    this.misdatos()
  },
  methods:{
    crear(){
      if (this.boolcrear)
      this.$api.post('/materia',this.materia).then(res=>{
        this.materia={}
        this.misdatos()
      })
      else
      this.$api.put('/materia/'+this.materia.id,this.materia).then(res=>{
        this.materia={}
        this.misdatos()
        this.boolcrear=true
      })
    },
    misdatos(){
      this.$api.get('/materia').then(res=>{
        this.materias=res.data
      })
    },
    eliminar(materia){
      if (confirm("Seguro de eliminar?"))
      this.$api.delete('/materia/'+materia.id).then(res=>{
        this.misdatos()
      })
    },
    modificar(materia){
      this.materia=materia
      this.boolcrear=false
    }
  }
}
</script>

<style scoped>

</style>
