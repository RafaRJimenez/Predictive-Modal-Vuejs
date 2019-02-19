<template>
    <div>
      <button type="button" class="btn btn-outline-primary" @click="showModal">Crear incidencia</button>
      <b-modal size="lg" id="modal" ref="myModalRef" hide-footer title="Incidencia">
        <div :class="{showAlert: finished}" class="alert alert-success" role="alert">
          <h4 class="alert-heading">Inicidencia Registrada</h4>
          <p> La incidencia ha sido registrada Correctamente y se procederá al cierre de la ventana</p>
        </div>
        <pp-information></pp-information>
        <hr>
        <div class="row text-left">
          <div class="col">Fecha/hora <br>
             <input v-model="time" id="datetime" type="datetime-local">
          </div>
          <div class="col">Severidad <br>
            <select @click="checkVar" v-model="severity" class="custom-select custom-select-sm">
              <option selected>Menor</option>
              <option>Mayor</option>
              <option>Grave</option>
            </select>
          </div>
          <div class="col">Tipo Incidencia <br>
            <select v-model="kind" class="custom-select custom-select-sm">
              <template v-if="severity == 'Menor'">
                <option selected>Tipo 1</option>
                <option>Tipo 2</option>
                <option>Tipo 3</option>
              </template>
              <template v-if="severity == 'Mayor'">
                <option selected>Tipo 4</option>
                <option>Tipo 5</option>
                <option>Tipo 6</option>
              </template>
              <template v-if="severity == 'Grave'">
                <option selected>Tipo 7</option>
                <option>Tipo 8</option>
                <option>Tipo 9</option>
              </template>
            </select>
          </div>
          <hr>
        </div>
        <div class="row text-left">
            <div class="col">Apertura <br>
              <input  @dblclick="setDateTime('time2')" v-model="time2" id="datetime" type="datetime-local">
              <div class="speech-bubble">Pulsa dos veces para obtener la fecha actual</div>
            </div>
            <div class="col">Abierta por <br>
            <pp-opened-by :removeOpener="removeOpener" @clicked="onClickChild"></pp-opened-by>
             <div class="speech-bubble">Debes utilizar uno de los siguientes nombres registrados: <b> 'Melissa', 'Ronda', 'Nola', 'Olivia' o 'Mae' </b>. La prebúsqueda se inicia pulsando los 3 primeros carácteres. Es seleccionado pulsando sobre él.</div>
            </div>
            <div class="col">Notificar a <br>
              <input type="text" v-model="adviceTo" class="form-control" name="">
            </div>
        </div>
        <div class="row text-left">
          <div class="container-fluid notas">
            Notas <br>
            <yimo-vue-editor :langs='en' class='clearfix notass' v-model='text'></yimo-vue-editor>
          </div>
        </div>
        <div class="row text-left">
          <div class="col">Cierre <br>
            <input @dblclick="setDateTime('time3')" v-model="time3" id="datetime" type="datetime-local">
          </div>
          <div class="col">Cerrada por <br>
            <pp-opened-by :removeOpener="removeOpener"></pp-opened-by>
          </div>
        </div>
        <div class="row text-left">
          <div class="col">
            Notas <br>
            <textarea class="notes2"></textarea>
          </div>
        </div>
        <div class="row">
          <div class="col">
            <b-btn variant="outline-danger" @click="hideModal">Eliminar incidencia</b-btn>
          </div>
          <div class="col">
            <button type="button" class="btn btn-outline-primary" @click="registerData">Registrar incidencia</button>
          </div>
        </div>
      </b-modal>
  </div>
</template>

<script>
import Information from './Information.vue'
import OpenedBy from './OpenedBy.vue'
import YimoVueEditor from 'yimo-vue-editor'

export default {
  name: 'CreateIncidence',
  data () {
    return {
      removeOpener: false,
      severity: '',
      kind: '',
      text: '',
      actualTime: '',
      time: '',
      time2: '',
      time3: '',
      finished: false,
      openedBy: '',
      adviceTo: '',
    }
  },
  created () {
    this.getTime()
    setInterval(() => {
        this.getTime()
        }, 30000)
  },
  methods: {
    getTime (){
    let today = new(Date)
    let day = today.getDate()
    let month = today.getMonth() + 1
    let year = today.getFullYear()
    let hour = today.getHours()
    let minute = today.getMinutes()
    if (day < 10) {
      day = '0' + day
    }
    if (month < 10) {
      month = '0' + month
    }
    if (hour < 10) {
      hour = '0' + hour
    }
    if (minute < 10) {
      minute = '0' + minute
    }
    this.actualTime = `${year}-${month}-${day}T${hour}:${minute}`
    this.time = this.actualTime
    },
    showModal () {
      this.$refs.myModalRef.show()
    },
    hideModal () {
      this.$refs.myModalRef.hide()
      this.finished = false
          this.severity = ''
          this.kind = ''
          this.text = ''
          this.time2 = ''
          this.time3 = ''
          this.adviceTo = ''
          this.removeOpener = !this.removeOpener
    },
    checkVar () {
      console.log(this.selected)
    },
    onClickChild (value) {
      this.openedBy = value
    },
    setDateTime (timeChange) {
      if (timeChange === 'time2') {
        this.time2 = this.actualTime
      } else {
        this.time3 = this.actualTime
      }
    },
    registerData () {
      if (this.time === '') {
        alert('Debes indicar la fecha y la hora de la incidencia')
      } else if (this.severity === '') {
        alert('Debes indicar la serveridad')
      } else if (this.kind === '') {
        alert('Debes indicar el tipo de incidencia')
      } else if (this.time2 === '') {
        alert('Debes indicar la fecha y la hora de la apertura')
      } else if (this.openedBy === '') {
        alert('Debes indicar quién ha abierto la incidencia')
      } else if (this.adviceTo === '' && this.severity === 'Grave') {
        alert('Debes indicar a quién debe ser notificado')
      } else {
        this.finished = true
        setTimeout(() => {
        this.hideModal()
        }, 3000)
      }
    }
  },
  components: {
    ppInformation: Information,
    ppOpenedBy: OpenedBy,
    YimoVueEditor: YimoVueEditor
  }

}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>

.modal{
 width: 100%;
}

.alert{
  position: absolute;
  z-index: 50;
  margin-left: 1rem;
  width: 90%;
  top: 30%;
  display: none;
  transition: all 3000ms;
}


.showAlert{
  position: absolute;
  z-index: 50;
  margin-left: 1rem;
  width: 90%;
  top: 30%;
  display: block;
  transition: all 3000ms;
}

.yimo-vue-editor {
    width: 100%;
    height: auto!important;
    min-height: 100px!important;
}

.row {
  padding: 10px 0;
}

.notes2{
  width: 100%;
  height: 80px;
}

.speech-bubble {
  position: relative;
  background: #00aabb;
  border-radius: .4em;
  margin-top: 1rem;
  color: black;
}

.speech-bubble:after {
  content: '';
  position: absolute;
  top: 0;
  left: 50%;
  width: 0;
  height: 0;
  border: 26px solid transparent;
  border-bottom-color: #00aabb;
  border-top: 0;
  border-right: 0;
  margin-left: -13px;
  margin-top: -26px;
}

</style>
