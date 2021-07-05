<template>
  <q-layout class="bg-teal-2" view="lHh Lpr Lff">
    <q-page-container>
      <q-page padding class="row items-center justify-center">
        <div class="row full-width">
          <div class="col-md-8 offset-md-2 col-xs-12 q-pa-md">
            <q-card class="text-blue-grey-14">
              <div class="row items-center">
                <div class="col-md-6 col-sm-12 col-xs-12">
                  <div class="row q-pt-md q-pb-md">
                    <div class="col-md-8 offset-2">
                      <q-img src="https://media.istockphoto.com/vectors/cinema-and-movie-time-vector-id640312764?k=6&m=640312764&s=170667a&w=0&h=-7bvtWDCxwAttItJKIIVkbMaiUpqnxv23a0iCIx2pUI="></q-img>
                    </div>
                  </div>
                </div>
              <div class="col-md-6">
                <q-card-section>
                  <div class="text-h4">MovieAPP</div>
                  <div class="text-h8">Login Your Account</div>
                </q-card-section>
                <q-form
                @submit="login"
                >
                <q-card-section>
                  <q-input
                  v-model="username"
                  label="Username"
                  />
                  <q-input
                  type="password"
                  v-model="password"
                  label="Password"
                  />
                </q-card-section>
                <q-card-sections class="q-gutter-md">
                  <q-btn class="full-width" type="submit" unelevated color="primary" label="Login"/>
                  <q-btn class="full-width q-mt-md" :to="{ name: 'registerPage' }" flat unelevated color="primary" label="Register"/>
                </q-card-sections>
                </q-form>
              </div>
                </div>
            </q-card>
          </div>
        </div>
      </q-page>
    </q-page-container>
  </q-layout>
</template>
<script>
export default {
  data () {
    return {
      username: null,
      password: null
    }
  },
  methods: {
    login () {
      this.$axios.post('user/login', {
        username: this.username,
        password: this.password
      }).then(res => {
        if (res.data.sukses) {
          this.$showNotif(res.data.pesan, 'positif')
          this.$q.localStorage.set('dataUser', res.data.data)
          if (res.data.data.level === 1) {
            this.$router.push({ name: 'dashboardAdmin' })
          } else {
            this.$router.push({ name: 'homeUser' })
          }
        } else {
          this.$showNotif(res.data.pesan, 'negative')
        }
      })
    }
  }
}
</script>
