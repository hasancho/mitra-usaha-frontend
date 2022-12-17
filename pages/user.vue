<template lang="">
  <div>
    <v-card class="mt-10 pb-5">
      <v-data-table
        :headers="headers"
        :items="users"
        sort-by="calories"
        class="elevation-1"
      >
        <template v-slot:top>
          <v-toolbar flat>
            <v-toolbar-title>Data User</v-toolbar-title>
            <v-spacer></v-spacer>
            <v-dialog max-width="500px">
              <template v-slot:activator="{}">
                <!-- <v-btn
                  color="primary"
                  dark
                  class="mb-2"
                  v-on:click="formVisibility('Tambah', true, false)"
                >
                  New Item
                </v-btn> -->
              </template>
            </v-dialog>
          </v-toolbar>
        </template>
      </v-data-table>
    </v-card>
  </div>
</template>
<script>
export default {
  data: () => ({
    users: [],
    headers: [
      {
        text: "Username",
        align: "start",
        sortable: false,
        value: "username",
      },
      { text: "Email", value: "email" },
    ],
  }),

  mounted() {
    this.getUsers();
  },

  methods: {
    async getUsers() {
      const users = await this.$axios("/api/auth/user");
      this.users = users.data;
      console.log(this.users);
    },
  },
};
</script>
<style lang=""></style>
