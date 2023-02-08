<template>
    <v-col cols="12">
        <v-row class="renglon">
        <v-data-table
        :headers="headers"
        :items="usuarios"
        :items-per-page="5"
        class="elevation-1"
        style="width: 100%;"
     >
    <template #[`item.actions`]="{ item }">
     <v-row class="renglon">
        <v-col cols="6">
         <v-btn icon color="yellow">
           <v-icon>mdi-human-edit</v-icon>
         </v-btn>
        </v-col>
        <v-col cols="6">
            <v-btn icon color="red" @click="dialogUser(item)">
              <v-icon>mdi-eraser</v-icon>
            </v-btn>
        </v-col>

     </v-row>
    </template>
    </v-data-table>
    </v-row>
    <v-row class="renglon">
        <v-btn block color="purple" @click="open">
          agregar nuevo usuario
        </v-btn> 
    </v-row>
    <v-dialog 
    v-model="openDialog" 
    width="800" 
    height="500"
    persistent
    >
        <v-card>
            <v-card-title>datos del usuario</v-card-title>
            <v-card-text>
                <v-form ref="formRegistro">
                    <v-text-field
                    v-model="name" 
                    type="text" 
                    placeholder="Name"
                     labe="Name">
                    </v-text-field>
                    <v-text-field
                    v-model="lastname" 
                    type="text" 
                    placeholder="Lastname"
                     labe="Lastname">
                    </v-text-field>
                    <v-text-field
                    v-model="email" 
                    type="text" 
                    placeholder="Email"
                     labe="Email">
                    </v-text-field>
                    <v-text-field
                    v-model="password" 
                    type="text" 
                    placeholder="Password"
                     labe="Password">
                    </v-text-field>

                </v-form>

            </v-card-text>
            <v-card-actions style="width: 100%; display: flex; flex-direction: column;">
                <v-row style="width: 100%; margin-top: 5px; margin-bottom: 10px;">
                    <v-btn block color="green">
                    Registrar
                </v-btn>
                </v-row>

                <v-row style="width: 100%; margin-top: 5px; margin-bottom: 10px;">
                    <v-btn block color="red" @click="openDialog = false">
                    Cancelar
                </v-btn>
                </v-row>
                
               
            </v-card-actions>

        </v-card>

    </v-dialog>
    <v-dialog
    v-model="openDialogErase" 
    width="800" 
    height="500"
    persistent
    >
    <v-card>
        <v-card-title>Borrar usuario</v-card-title>
        <v-card-text>realemnte lo borraras</v-card-text>
        <v-card-actions>
            <v-btn color="red" @click="openDialogErase = false"> Cancelar</v-btn>
            <v-spacer></v-spacer>
    <v-btn color="orange">Borrar</v-btn>
    
    
        </v-card-actions>
    </v-card>

</v-dialog>
    </v-col>
    
</template>

<script>


    export default {
    data () {
        return {
            usuarios: [],
            headers: [
            
                {
                    text:'Nombre',
                    align: 'center',
                    sortable: true,
                    value: 'name'
                },{
                    text:'Apellidos',
                    align: 'center',
                    sortable: true,
                    value: 'lastname'
                },
                {
                    text:'Correo',
                    align: 'center',
                    sortable: true,
                    value: 'email'
                },
                {
                    text:'Creacion',
                    align: 'center',
                    sortable: true,
                    value: 'date'
                },{
                    text:'Acciones',
                    align: 'center',
                    sortable: true,
                    value: 'actions'
                }
            ],
            openDialog: false,
            name: '',
            lastname: '',
            email: '',
            password: '',
            idEraserUser: '',
            openDialogErase: false,
            admin: 'Ramon'

        }
    },
    mounted () {
       this.loadUsers()
    },
    methods: {
        async loadUsers() {
            const config = {
                headers:{
                  'Content-Type': 'application/json;charset=UTF-8',
                  'Access-Control-Allow-Origin': '*'
                }
            }
            await this.$axios.get('/getallusers', config)
            .then((res) => {
                console.log('res', res)
            })
            .catch((error) => {
                console.log('error', error)
            })
        },
        open () {
            this.openDialog = true
        },
        async registraUsuario () {
            const config = {
                headers: {
                    'Content-Type': 'application/json;charset=UTF-8',
                  'Access-Control-Allow-Origin': '*'
                }
            }
            const usuarioNuevo = {
                name: this.name,
                lastname: this.lastname,
                email: this.email,
                password: this.password
            }
            await this.$axios.post('/register', usuarioNuevo, config)
            .then((res) => {
                console.log('res', res)
                if(res.data.error == null){
                    this.openDialog = false
                }
            })
            .catch((error) => {
                console.log('error', error)
            })
        },
        async eraseUser (item) {
            console.log(item)
            if (this.admin !== 'Ramon') {
                const config = {
                headers: {
                    'Content-Type': 'application/json;charset=UTF-8',
                  'Access-Control-Allow-Origin': '*'
                }
            }
            const usuario = {
                id: this.idEraserUser
            }
            await this.$axios.post('/user/eraseusers', usuario, config)
            .then((res) => {
                console.log(res)
                if (res.data.message == 'usuario borrado') {
                    this.loadUsers()
                    this.openDialogErase = false
                }
            })
            .catch((error) => {
                console.log(error)
            })
            }
        },
        dialogUser(item){
            this.idEraserUser = item._id
            this.openDialogErase = true
        }
    }
}

</script>
<style scoped>
.renglon{
    width: 100%;
    margin-top: 20px;
    margin-bottom: 20px;
}
</style>