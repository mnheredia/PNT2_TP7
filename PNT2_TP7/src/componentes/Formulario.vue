<template>

  <section>
    <div class="jumbotron">
      <h2>Punto 1 - Formulario</h2>
      <hr>

      <vue-form :state="formState" @submit.prevent="enviar()">

        <!-- CAMPO NOMBRE -->
        <validate tag="div">
          <label for="nombre">Nombre</label>
          <input type="text" id="nombre" name="nombre" class="form-control" autocomplete="off"
            v-model.trim="formData.nombre" required :minlength="nombreMinLength" :maxlength="nombreMaxLength"
            no-espacios>
          <field-messages name="nombre" show="$dirty">
            <div slot="required" class="alert alert-danger mt-1">Campo obligatorio</div>
            <div slot="no-espacios" class="alert alert-danger mt-1">Error: No se permiten espacios intermedios en este
              campo</div>
            <div slot="minlength" class="alert alert-danger mt-1">Error: Este campo requiere como mínimo {{
                nombreMinLength
            }} caracteres</div>
            <div v-if="formData.nombre.length == nombreMaxLength" class="alert alert-warning mt-1">Error: Este campo
              debe tener como máximo {{ nombreMaxLength }} caracteres</div>
          </field-messages>
        </validate>
        <br>

        <!-- CAMPO EDAD -->
        <validate tag="div">
          <label for="edad">Edad</label>
          <input type="number" id="edad" name="edad" class="form-control" autocomplete="off"
            v-model.number="formData.edad" required :min="edadMin" :max="edadMax">
          <field-messages name="edad" show="$dirty">
            <div slot="required" class="alert alert-danger mt-1">Campo obligatorio</div>
            <div slot="min" class="alert alert-danger mt-1">Error: La edad mínima es de {{ edadMin }} años</div>
            <div slot="max" class="alert alert-danger mt-1">Error: La edad máxima permitida es de {{ edadMax }} años
            </div>
          </field-messages>
        </validate>
        <br>

        <!-- CAMPO EMAIL -->
        <validate tag="div">
          <label for="email">Email</label>
          <input type="email" id="email" name="email" class="form-control" autocomplete="off"
            v-model.trim="formData.email" required>
          <field-messages name="email" show="$dirty">
            <div slot="required" class="alert alert-danger mt-1">Campo obligatorio</div>
            <div slot="email" class="alert alert-danger mt-1">Error: Email no válido</div>
          </field-messages>
        </validate>
        <br>

        <button class="btn btn-success my-3" :disabled="formState.$invalid" type="submit">Enviar</button>
      </vue-form>
    </div>

  </section>




</template>

<script>

export default {
  name: 'form',
  props: [],
  mounted() {

  },
  data() {
    return {
      url: 'https://637414830bb6b698b61b20a5.mockapi.io/turnos/api/users/',
      users: [],
      formData: this.getInitialData(),
      formState: {},
      nombreMinLength: 5,
      nombreMaxLength: 15,
      edadMin: 18,
      edadMax: 120

    }
  },
  methods: {
    getInitialData() {
      return {
        nombre: '',
        edad: '',
        email: ''
      }
    },

    clear() {
      this.formData = {
        nombre: '',
        edad: '',
        email: ''
      }
      this.formState._reset()
    },
    async enviar() { //async/await
      console.log({ ...this.formData })
      let usersPost = {
        nombre: this.formData.nombre,
        edad: this.formData.edad,
        email: this.formData.email
      }
      this.formData = this.getInitialData()
      this.formState._reset()


      try {
        let { data: user } = await this.axios.post(this.url, usersPost, { 'content-type': 'application/json' })
        //console.log(user)
        this.users.push(user)
      }
      catch (error) {
        console.error('Error en postUsers', error.message)
      }

    },
  },
  computed: {

  }
}


</script>

<style>
.jumbotron {
  background-color: rgb(180, 221, 180);
  color: white;
}

hr {
  background-color: #eee;
}

pre {
  color: white;
}
</style>
