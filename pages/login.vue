<template lang="">
  <div>
    <v-form>
      <v-card style="width: 500px; border-radius: 15px">
        <v-card-title>Login</v-card-title>
        <v-divider></v-divider>
        <v-col cols="8">
          <v-text-field label="Username" outlined></v-text-field>
          <v-text-field label="Password" outlined></v-text-field>
        </v-col>
        <v-divider></v-divider>
        <v-btn class="ma-5" color="primary">Login</v-btn>
      </v-card>
    </v-form>
  </div>
</template>
<script>
export default {
  layout: "plain",
};
</script>
<style lang=""></style>
