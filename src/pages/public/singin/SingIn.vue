<template>
  <v-container fluid>
    <v-row v-if="this.$route.params.context">
      <v-col cols="12" class="pb-0 pt-0">
        <breadcrumb :items="breadcrumbItems"/>
      </v-col>
    </v-row>
    <v-row
        justify="center"
        align="center"
    >
      <v-col
          class="xs"
          lg="4"
          md="6"
          sm="12"
          style="margin: 0 auto"
      >
        <v-card
            elevation="5"
            class="mx-auto mt-5"
        >
          <v-card-text>
            <div style="padding: 15px;border-bottom-left-radius: 0px;border-bottom-right-radius: 0px;text-align: center;position: relative">
              <v-row
                  justify="center"
                  class="mt-0"
              >
                <img  style="height: 80px;width: 70px;"
                      src="./../../../assets/logo.svg"
                      alt="Logo"
                >
              </v-row>
              <h2
                  class="text-md-center text-lg-center form-title mt-7"
              >
                {{ $t('signin') }}
              </h2>
              <p class="font-weight-regular text-md-center text-lg-center body-1 mt-1">
                Please fill in all the details
              </p>
            </div>

            <v-form
                ref="form"
                v-model="valid"
                lazy-validation
            >
              <v-text-field
                  dense
                  prepend-inner-icon="mdi-email"
                  v-model="user.email"
                  :rules="rules.required"
                  label="Email"
                  required
                  name="email"
                  outlined
              >
              </v-text-field>

              <v-text-field
                  id="otp"
                  v-model="user.password"
                  dense
                  :type="showPassword ? 'text' : 'password'"
                  outlined
                  :append-icon=" showPassword? 'mdi-eye' : 'mdi-eye-off'"
                  required
                  :rules="rules.password"
                  prepend-inner-icon="mdi-security"
                  name="password"
                  label="Password"
                  @keyup.enter.native="validateForm"
                  @click:append="showPassword = !showPassword"
              />
            </v-form>
            <p  class="pa-0" style="text-align: right;margin-top:-10px"><a @click="resetPassword">Forget Password? Reset</a></p>
            <v-btn
                class="mt-5 mb-5"
                block
                :loading="submit"
                :disabled="submit"
                color="success"
                @click="validateForm"
            >
              Sign In
              <template v-slot:loader>
                <span class="custom-loader">
                  <v-icon light>mdi-cached</v-icon>
                </span>
              </template>
            </v-btn>
          </v-card-text>
        </v-card>

      </v-col>
    </v-row>

  </v-container>
</template>

<script>
import {getRandomString} from "../../../helpers/RandomString";
import Breadcrumb from "../../../components/breadcrumb/Breadcrumb";
export default {
  name:"user-sign-in-form",
  components:{
    'breadcrumb':Breadcrumb,
  },
  data () {
    return {
      breadcrumbItems:[
        {
          text: 'Home',
          disabled: false,
          to: {name:'Dashboard'},
        },
        {
          text: 'Sign In',
          disabled: true,
          to: {name:'PageSignIn'},
        },
      ],
      submit: false,
      valid: true,
      showPassword: false,
      user: {
        email:"johndoe@example.com",
        password: "demo",
      },
      rules: {
        email: [
          v => !!v || 'Email is required',
        ],
        required: [
          v => !!v || 'This field is required',
        ],
        password: [
          v => !!v || 'Password is required',
        ],
      },
    }
  },
  mounted() {
  },
  methods: {
    getLogin () {
      this.$Progress.start()
      this.submit = false
      this.$Progress.finish();
      this.$refs.form.resetValidation();
      console.log("token",getRandomString(100))
      this.$store.dispatch('setAuthenticatedData', { token: getRandomString(100), user:{name:'John Doe',email:'vue-admin-template@domain.com'} });
      this.$store.dispatch('setActiveNavigation','Dashboard')
      this.$router.push({ name: 'Dashboard'})
    },
    resetPassword(){
      console.log("param",this.$route.params)
      if(!this.$route.params.context){
        this.$router.push({name:'ForgetPassword'})
      }
    },
    validateForm () {
      if(!this.$route.params.context){
        if (this.$refs.form.validate()) {
          this.getLogin()
        }
      }

    },

  },

}
</script>
<style>
</style>
