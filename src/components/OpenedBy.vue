<template>
  <div>
    <input id="inputName" type="text" class="form-control" name="" v-model="search">
      <div v-for="name in filteredList">
      <a @click="setName(name.first_name); onClickButton()" v-if="nameChecked&&search.length > 2 ">
        {{ name.first_name }}
      </a>
    </div>
  </div>
</template>

<script>
export default {
  name: 'OpenedBy',
  data () {
    return {
      search: '',
      nameChecked: true,
      names: [
        {'first_name': 'Melissa'},
        {'first_name': 'Ronda'},
        {'first_name': 'Nola'},
        {'first_name': 'Baryram'},
        {'first_name': 'Sherilyn'},
        {'first_name': 'Mae'},
        {'first_name': 'Isacco'},
        {'first_name': 'Rhys'},
        {'first_name': 'Lem'},
        {'first_name': 'Colene'},
        {'first_name': 'Rorie'},
        {'first_name': 'Tanitansy'},
        {'first_name': 'Olivia'},
        {'first_name': 'Stillmann'},
       ]
    }
  },
  methods: {
    setName: function (nombre) {
      this.search = nombre
      this.nameChecked = false
    },
    onClickButton (event) {
      this.$emit('clicked', this.search)
    }

  },
  watch: {
     removeOpener: function(){
       this.search= ''
     }
    },
  computed: {
    filteredList () {
      if (this.search.length === 0) {
        this.$emit('clicked', this.search)
        this.nameChecked = true
      }
      return this.names.filter(name => {
        return name.first_name.toLowerCase().includes(this.search.toLowerCase())
      })
    }
  },
  props: ['removeOpener'],
}
</script>

<style scoped>

</style>
