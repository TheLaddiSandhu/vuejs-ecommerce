<template>
  <div class="auth-wrapper auth-v1">
    <div class="auth-inner">
      <v-card class="auth-card">
        <!-- logo -->
        <v-card-title class="d-flex align-center justify-center py-7">
          <router-link
            to="/"
            class="d-flex align-center"
          >
            <v-img
              :src="require('@/assets/logo/logo.svg')"
              max-height="30px"
              max-width="30px"
              alt="logo"
              contain
              class="me-3 "
            ></v-img>

            <h2 class="text-2xl font-weight-semibold">
              VueKart
            </h2>
          </router-link>
        </v-card-title>

        <!-- title -->
        <v-card-text>
          <p class="text-2xl font-we bight-semibold text--primary mb-2">
            Welcome to VueKart!
          </p>
          <p class="mb-2">
            Please sign-in to your account and explore the world
          </p>
        </v-card-text>

        <!-- login form -->
        <v-card-text>
          <v-form>
            <v-text-field
              v-model="email"
              outlined
              label="Email"
              placeholder="john@example.com"
              hide-details
              class="mb-3"
            ></v-text-field>

            <v-text-field
              v-model="password"
              outlined
              :type="isPasswordVisible ? 'text' : 'password'"
              label="Password"
              placeholder="············"
              :append-icon="isPasswordVisible ? icons.mdiEyeOffOutline : icons.mdiEyeOutline"
              hide-details
              @click:append="isPasswordVisible = !isPasswordVisible"
            ></v-text-field>

            <div class="d-flex align-center justify-space-between flex-wrap">
              <v-checkbox
                label="Remember Me"
                hide-details
                class="me-3 mt-1"
              >
              </v-checkbox>

              <!-- forgot link -->
              <a
                href="javascript:void(0)"
                class="mt-1"
              >
                Forgot Password?
              </a>
            </div>

            <v-btn
              block
              color="primary"
              class="mt-6"
              v-on:click="login()"
            >
              Login
            </v-btn>
            <p v-if="message">{{message}}</p>
          </v-form>
        </v-card-text>

        <!-- create new account  -->

      </v-card>
    </div>

    <!-- background triangle shape  -->
    <img
      class="auth-mask-bg"
      height="173"
      :src="require(`@/assets/images/misc/mask-${$vuetify.theme.dark ? 'dark':'light'}.png`)"
    >

    <!-- tree -->
    <v-img
      class="auth-tree"
      width="247"
      height="185"
      src="@/assets/images/misc/tree.png"
    ></v-img>

    <!-- tree  -->
    <v-img
      class="auth-tree-3"
      width="377"
      height="289"
      src="@/assets/images/misc/tree-3.png"
    ></v-img>
  </div>
</template>

<script>
// eslint-disable-next-line object-curly-newline
import { mdiFacebook, mdiTwitter, mdiGithub, mdiGoogle, mdiEyeOutline, mdiEyeOffOutline } from '@mdi/js'
import { ref } from '@vue/composition-api'

export default {
  setup() {
    const isPasswordVisible = ref(false)
    const email = ref('')
    const password = ref('')
    const socialLink = [
      {
        icon: mdiFacebook,
        color: '#4267b2',
        colorInDark: '#4267b2',
      },
      {
        icon: mdiTwitter,
        color: '#1da1f2',
        colorInDark: '#1da1f2',
      },
      {
        icon: mdiGithub,
        color: '#272727',
        colorInDark: '#fff',
      },
      {
        icon: mdiGoogle,
        color: '#db4437',
        colorInDark: '#db4437',
      },
    ]

    return {
      isPasswordVisible,
      email,
      password,
      socialLink,

      icons: {
        mdiEyeOutline,
        mdiEyeOffOutline,
      },
    }
  },
  data: () => ({
    message: null,
  }),
  methods: {
    login() {
      if(this.email != '' && this.password != ''){
        this.axios
        .post("http://localhost/ecommerce-service/api/user.php?action=loginUser", {
          email: this.email,
          password: this.password
        })
        .then((response) => { 
          console.log(response);
          if(response.data == '' ){
            this.message = "Invalid username or password";
          }else {
            this.message = null;
            this.$store.commit('setUserIdSession', response.data.id);
            this.$router.push('/');
          }
        })
        .catch((error) => {
          console.log(error);
          this.message = "Invalid username or password";
        });
      }else {
        this.message = "Email and password required!"
      }
    },
    checkIfEmailAndPasswordCorrect(email, password){
      let index = this.users.findIndex((user) => user.email == email && user.password == password);
      return index;
    }
  },
  computed: {
    users() {
      return this.$store.state.users;
    }
  }
}
</script>

<style lang="scss">
@import '~@/plugins/vuetify/default-preset/preset/pages/auth.scss';
</style>
