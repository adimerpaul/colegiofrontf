<template>
  <q-page>
    <div class="row">
      <div class="col-12">
        <q-form @submit.prevent="crear">
          <div class="row q-pa-xs">
            <div class="col-3">
              <q-input outlined dense label="nombre" required v-model="docente.nombre"/>
            </div>
            <div class="col-3">
              <q-input type="date" outlined dense label="fechanac" required v-model="docente.fechanac"/>
            </div>
            <div class="col-3">
              <q-input outlined dense label="sueldo" required v-model="docente.sueldo"/>
            </div>
            <div class="col-3 flex flex-center">
              <q-btn type="submit" outlined dense icon="send" :color="boolcrear?'positive':'yellow'" :label="boolcrear?'Insertar':'Modificar'"/>
            </div>
          </div>
        </q-form>
      </div>
      <div class="col-12">
        <q-table title="Mis cursos" :columns="columns" :rows="docentes" dense>
          <template v-slot:body="props">
            <q-tr :props="props">
              <q-td key="id" :props="props">
                <q-badge>{{props.row.id}}</q-badge>
              </q-td>
              <q-td key="nombre" :props="props">
                <q-badge color="teal" >{{props.row.nombre}}</q-badge>
              </q-td>
              <q-td key="fechanac" :props="props">
                <q-badge color="negative">{{props.row.fechanac}}</q-badge>
              </q-td>
              <q-td key="activo" :props="props">
                <q-badge color="negative">{{props.row.activo}}</q-badge>
              </q-td>
              <q-td key="sueldo" :props="props">
                <q-badge color="negative">{{props.row.sueldo}}</q-badge>
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
import {date} from 'quasar'
export default {
  data(){
    return {
      docentes:[],
      docente:{fechanac:date.formatDate(Date.now(),'YYYY-MM-DD')},
      boolcrear:true,
      columns:[
        {name:'id',label:'#',field:'id'},
        {name:'nombre',label:'Nombre',field:'nombre'},
        {name:'fechanac',label:'fechanac',field:'fechanac'},
        {name:'activo',label:'activo',field:'activo'},
        {name:'sueldo',label:'sueldo',field:'sueldo'},
        {name:'opciones',label:'Opciones',field:'opciones'},
      ]
    }
  },
  created() {
    this.misdatos()
  },
  methods:{
    crear(){
      this.$q.loading.show()
      if (this.boolcrear)
        this.$api.post('/docente',this.docente).then(res=>{
          this.docente={fechanac:date.formatDate(Date.now(),'YYYY-MM-DD')}
          this.misdatos()
          this.$q.loading.hide()
        })
      else
        this.$api.put('/docente/'+this.docente.id,this.docente).then(res=>{
          this.docente={fechanac:date.formatDate(Date.now(),'YYYY-MM-DD')}
          this.misdatos()
          this.boolcrear=true
        })
    },
    misdatos(){
      this.$api.get('/docente').then(res=>{
        this.docentes=res.data
        console.log(this.docentes)
      })
    },
    eliminar(docente){
      if (confirm("Seguro de eliminar?"))
        this.$api.delete('/docente/'+docente.id).then(res=>{
          this.misdatos()
        })
    },
    modificar(docente){
      this.docente=docente
      this.boolcrear=false
    }
  }
}
</script>

<style scoped>

</style>
