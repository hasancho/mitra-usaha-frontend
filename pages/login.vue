<template lang="" color="primary">
  <div>
    <v-form @submit.prevent="loginUser">
      <v-card style="width: 500px; margin-top: 60px; margin-left: 100px">
        <!-- <v-card-title>Login</v-card-title> -->
        <v-toolbar dark color="pink" style="text-align: center">
          <v-toolbar-title>MITRA USAHA</v-toolbar-title>
        </v-toolbar>
        <v-divider></v-divider>
        <v-col cols="8">
          <v-text-field
            v-model="login.username"
            label="Username"
            outlined
          ></v-text-field>
          <v-text-field
            v-model="login.password"
            label="Password"
            type="password"
            outlined
          ></v-text-field>
        </v-col>
        <v-divider></v-divider>
        <v-btn class="ma-5" type="submit" color="pink" style="color: white"
          >Login</v-btn
        >
      </v-card>
    </v-form>
  </div>
</template>
<script>
export default {
  layout: "plain",
  auth: false,
  data() {
    return {
      login: {
        username: "",
        password: "",
      },
    };
  },
  methods: {
    async loginUser() {
      try {
        let response = await this.$auth.loginWith("local", {
          data: this.login,
        });
        console.log(response);
        this.$router.push("/");
      } catch (err) {
        console.log(err);
      }
      // try {
      //   const data = await this.$auth.loginWith("local", {
      //     data: {
      //       username: this.username,
      //       password: this.password,
      //     },
      //   });
      //   console.log(data);
      //   this.$router.push("/");
      // } catch (error) {
      //   console.log(error);
      // }
    },
  },
};
</script>
