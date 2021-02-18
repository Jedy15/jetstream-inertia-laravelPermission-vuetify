<template>
  <v-app>
    <v-row>
      <v-card class="info d-none d-md-flex align-center justify-center col-lg-7 col-xl-6 col-12">
        <v-container>
          <v-row class="justify-center">
            <v-col cols="8" xl="5">
              <div>
                <h2 class="display-1 white--text font-weight-medium">
                  Sistemas Informaticos Jedy
                </h2>
              </div>
            </v-col>
          </v-row>
        </v-container>
      </v-card>
      <v-card class="d-flex align-center col-lg-5 col-xl-6 col-12">
        <v-container>
          <div class="pa-7 pa-sm-12">
            <v-row>
              <v-col cols="12" lg="9" xl="6">
                <div>
                  <v-alert v-if="hasErrors" type="error">
                    <h3>¡D'oh! Algo salio mal.</h3>
                    <ul>
                        <li v-for="(error, key) in errors" :key="key">
                            {{  error }}
                        </li>
                    </ul>
                  </v-alert>
                </div>
                <h2 class="font-weight-bold mt-4 blue-grey--text text--darken-2">
                  Iniciar Sesión
                </h2>
                <v-form @submit.prevent="submit">
                  <v-text-field
                    label="E-mail"
                    outlined
                    class="mt-4"
                    v-model="form.email" 
                    required 
                    autofocus
                  ></v-text-field>
                  <v-text-field
                    label="Contraseña"
                    type="password"
                    outlined
                    v-model="form.password" 
                    required 
                    autocomplete="current-password"
                  ></v-text-field>
                  <v-btn
                    class="mr-4"
                    block
                    color="primary"
                    :disabled="form.processing"
                    type="submit"
                  >
                    Ingresar
                  </v-btn>
                </v-form>
              </v-col>
            </v-row>
          </div>
        </v-container>
      </v-card>
    </v-row>
  </v-app>
</template>

<script>
  export default {
      props: {
          canResetPassword: Boolean,
          status: String
      },

      data() {
          return {
              form: this.$inertia.form({
                  email: '',
                  password: '',
                  remember: false
              })
          }
      },

      methods: {
          submit() {
              this.form
                  .transform(data => ({
                      ... data,
                      remember: this.form.remember ? 'on' : ''
                  }))
                  .post(this.route('login'), {
                      onFinish: () => this.form.reset('password'),
                  })
          }
      },

      computed: {
            errors() {
                return this.$page.props.errors
            },

            hasErrors() {
                return Object.keys(this.errors).length > 0;
            },
        }
  }
</script>
