<template>


    <div>
        <v-img class="mx-auto my-6" max-width="228" src="../static/logo1.jpg">
        </v-img>

        <v-card class="mx-auto pa-12 pb-8" elevation="8" max-width="448" rounded="lg">
            <v-form ref="form">
                <v-text-field v-model="logg.nombre" label="Nombre de Usuario" density="compact"
                    placeholder="Nombre de Usuario" prepend-inner-icon="mdi-account" variant="outlined"
                    :error-messages="nombreErrors" required>
                </v-text-field>

                <v-text-field v-model="logg.pass" label="Contraseña"
                    :append-inner-icon="visible ? 'mdi-eye-off' : 'mdi-eye'" :type="visible ? 'text' : 'password'"
                    density="compact" placeholder="Contraseña" prepend-inner-icon="mdi-lock-outline" variant="outlined"
                    @click:append-inner="visible = !visible" :error-messages="passErrors" required>
                </v-text-field>
            </v-form>

            <!-- Mensaje de error -->
            <v-alert v-if="errorMessage" type="error" class="mb-4">
                {{ errorMessage }}
            </v-alert>

            <v-btn class="mb-8" color="blue" size="large" variant="tonal" block @click="login">
                Log In
            </v-btn>
        </v-card>
    </div>

</template>


<script>
export default {
    data: () => ({
        visible: false,
        errorMessage: '', // Almacena el mensaje de error general
        nombreErrors: [], // Almacena los errores específicos del campo 'nombre'
        passErrors: [], // Almacena los errores específicos del campo 'pass'
        logg: {
            nombre: '',
            pass: ''
        }
    }),
    methods: {
        login() {
            this.validateFields();
            if (this.nombreErrors.length === 0 && this.passErrors.length === 0) {
                this.$axios.post('/auth/login', this.logg)
                    .then((response) => {
                        const {
                            ok,
                            userData,
                            mensage
                        } = response.data;

                        if (ok) {
                            console.log('Login exitoso:', userData);
                            this.errorMessage = 'Login exitoso'; // Limpia el mensaje de error si es exitoso
                        } else {
                            console.log(ok, " b")
                            this.errorMessage = mensage;
                        }
                    }).catch((error) => {
                        this.errorMessage = 'Error al iniciar sesión:', error
                        //this.errorMessage = 'Usuario o contraseña incorrecta';
                    });
            }
        },
        validateFields() {
            // Limpia los mensajes de error anteriores
            this.nombreErrors = [];
            this.passErrors = [];

            // Verifica si 'nombre' está vacío
            if (!this.logg.nombre) {
                this.nombreErrors.push('El nombre de usuario es obligatorio');
            }

            // Verifica si 'pass' está vacío
            if (!this.logg.pass) {
                this.passErrors.push('La contraseña es obligatoria');
            }
        }
    }
}
</script>