<template>
  <div class="jumbotron">
    <!-- ------------------ -->
    <!-- Temas de discusión -->
    <!-- ------------------ -->
    <div id="encabezado">
    <img src="../assets/inicio.png" width="900" height="130">
    <br><br>
    <div class= "text" align="right">
      <button id="crearUsuario" class="btn btn-success" @click="crearUsuario()">Crear usuario <b>+</b></button>
     </div>
      
    </div>
    <hr />
    <br>
    <div v-if="usuarios.length">
      <table class="table table-dark">
        <thead class="thead-dark">
          <tr :style="{color:'NAVY'}">
            <th scope="col">Nombre</th>
            <th scope="col">Edad</th>
            <th scope="col">Email</th>
            <th scope="col">Fecha de creación</th>
          </tr>
        </thead>
        <tr v-for="(usuario, index) in usuarios" :key="index">
          <td scope="col"> <p>{{ pasarAMayuscula(usuario.nombre) }}</p> </td>
          <td scope="col"> <p>{{ usuario.edad }}</p> </td>
          <td scope="col"> <a><b>{{ usuario.email }}</b></a> </td>
          <td scope="col"> <p>{{ formatearFechaHora(usuario.createdAt) }}</p> </td>                   
          <td scope="col"> <button id="eliminar" class="btn btn-danger" @click="eliminarUsuario(usuario.id)">Eliminar</button> </td>
        </tr>
      </table>
    </div>
    <div v-if ="!usuarios.length" class="alert alert-warning"> <h5>No hay usuarios creados</h5> </div>
    <div class="footer-copyright text-center py-3">© Diego Chiaradia TP6 </div>
    
  </div>
</template>

<script>
  import filters from '../filters'

  export default  {
    name: 'src-components-home',
    components: {
    },
    props: [],
    mixins: [filters],
    mounted () {
      this.getUsuarioFormAxios()
    },
    data () {
      return {
        usuarios: [],
        url: 'https://5fb95bc62f145f0016c3ce0b.mockapi.io/usuarios/'
      }
    },
    methods: {
      crearUsuario() {
        this.$router.push({ name: 'Form' })
        console.log("Entro al fomulario")
      },
      
      /* Pedido de datos almacenados en MockAPI */
      async getUsuarioFormAxios() {
        try {
          let res = await this.axios(this.url)
          this.usuarios = res.data
          console.log(res.data)
        } 
        catch(error) {
          console.log('HTTP GET ERROR', error)
        }
      },
      eliminarUsuario(id) {
          console.log('delete',id)

          this.axios.delete(this.url+id)
            .then(res => {
              let usuario = res.data
              console.log(usuario)
              let offset = this.usuarios.findIndex(usuario => usuario.id == id)
              this.usuarios.splice(offset,1)
            })
            .catch(error => console.log('HTTP DELETE ERROR', error))
      }
    },
    computed: {
      }
    
  }
</script>

<style scoped lang="css">
  #encabezado {
    display: inline;
  }
  .h2 {
    text-align: left;
  }
  #crearTema {
    float: right;
  }
</style>
