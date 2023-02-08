<template>
    <v-card width="500" color="blue lighten-4">
    <v-card-title class="title">Iniciar Sesion</v-card-title>
    <v-card-text>
      <v-row justify="center" align="center">
        <v-col cols="4" justify ="center" align-self="center">
          <img src="../../assets/images/1.jpg" class="imgLogin">
        </v-col>
        <v-col cols="8" justify ="center" align-self="center">
          <v-form ref="formLogin">
<v-text-field label="Correo Electronico"
               placeholder="Correo Electronico"
               v-model="correoElectronico"
               :rules="validarCorreo"
               />


               <v-text-field label="Contraseña"
               placeholder="Contraseña"
               v-model="contraseña"
               :rules="validarContraseña"
               />


        </v-form>
        </v-col>
      </v-row>
        
    </v-card-text>
    <v-card-actions>
        <v-btn
       color="black"
        class="btnLogin"
        rounded
        block
        @click="loginBackend"
        >
          Entrar
        </v-btn>
    </v-card-actions>    
    </v-card>
    
    </template>

<script>
export default{
data() {
return{
  
  correoElectronico: '',
  validarCorreo:[v => !v || /^\w+([.-]?\w+)*@\w+([.-]?\w+)*(\.\w{2,3})+$/.test(v) || 'Escribe un correo valido'],
  contraseña: '',
  validarContraseña:[value => value.length >= 6 || 'Min 6 caracteres']
}
},
methods:{
  async loginBackend () {
    const valid = this.$refs.formLogin.validate()
    if (valid) {
     const sendData = {
      email: this.correoElectronico,
      password: this.contraseña
     }
     await this.$auth.loginWith('local', {
      data: sendData
     }).then(async  (res) => {
      console.log('backend ', res)
      if (res.data.error == null) {
        this.$router.push('/dashboard')
      }
     }).catch((error) => {
      console.log('error', error)
     })
    }else{
      alert('llena los campos')
    }
    
  }
}
}
</script>


<style scoped>
  .cardLogin{
    background-color: black;
    border-radius:10px ;
    width: 500px;
    height: 300px;
  }
.imgLogin {
width: 100%;
height: 100%;
}
.btnLogin{
  background-color: purple!important;
}
.title{
  font-size: 30px;
  justify-content: center;
  color: rgb(102, 15, 129);
}
</style>