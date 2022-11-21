<template>

  <section class="src-componentes-http">
    <div class="jumbotron">
      <h2>AXIOS - THEN/CATCH - ASYNC/AWAIT </h2>
      <hr>
      
      <button class="btn btn-secondary my-3 mr-3" @click="clearData()">LIMPIAR DATOS</button>
      <button class="btn btn-info my-3 mr-3" @click="getUsersThenCatch()">AXIOS - THEN/CATCH</button>
      <button class="btn btn-success my-3 mr-3" @click="getUsers()">AXIOS - ASYNC/AWAIT</button> 
      


      <div v-if="users.length" class="table-responsive">
        <table class="table table-dark">
          <tr>
            <th>Nombre</th>
            <th>Edad</th>
            <th>Email</th>
          </tr>
          <tr v-for="user in users" :key="user.id">
            <td>{{ user.nombre }}</td>
            <td>{{ user.edad }}</td>
            <td>{{ user.email }}</td>
          </tr>
        </table>
        <h4 class="alert alert-primary">Se encontraron {{users.length}} usuarios</h4>
      </div>
      <h4 v-else class="alert alert-danger text-center">No se encuentran datos</h4>


    </div>
  </section>

</template>

<script lang="js">

  export default  {
    name: 'src-componentes-http',
    props: [],
    
    mounted () {
    },

    data () {
      return {
        url: 'https://637414830bb6b698b61b20a5.mockapi.io/turnos/api/users/',
        users: []
      }
    },

    methods: {
      
      getUsersThenCatch() {
          this.axios(this.url)
            .then( respuesta => {
              let { data : users } =  respuesta
              this.users = users
            })
            .catch( error => console.error(error) )
      },

      async getUsers(){
       
        try {
          
          let { data : users } = await this.axios(this.url)
          console.log(users)
          this.users = users
        }
        catch(error) {
          console.error('Error en getUsers', error.message)
        }
      },
      

      clearData(){
        this.users= [] 
      },
      
      
    },

    computed: {

    }
  }

</script>

<style >
  .jumbotron {
    background: rgb(180,221,180);
    color: white;
  }

  hr {
    background-color: #fff;
  }

  button {
    margin-right: 20px;
  }

</style>