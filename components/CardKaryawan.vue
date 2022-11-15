<template>
    <v-card class="ma-5">
      <v-card-text>
        <v-row>
          <v-col cols="12" md="6">
            <h2>{{ dataCustomer.npwp }}</h2>
            <h3>{{ dataCustomer.nama }}</h3>
            <h3>{{ dataCustomer.alamat }}</h3>
            <h3>{{ dataCustomer.no_telp }}</h3>
          </v-col>
          <v-col cols="12" md="6">
            <div class="float-right">
              <v-btn
                color="warning"
                v-on:click="$emit('edit', dataCustomer)"
                small
                >Edit</v-btn
              >
              <v-btn color="error" v-on:click="$emit('hapus', dataCustomer)" small
                >Hapus</v-btn
              >
            </div>
          </v-col>
        </v-row>
      </v-card-text>
    </v-card>
  </template>
  
  <script>
  export default {
    props: {
      dataCustomer: {
        type: Object,
      },
    },
    data: () => ({
      headers: [
        {
          text: "No",
          align: "start",
          sortable: false,
          value: "no",
        },
        { text: "NPWP", value: "npwp" },
        { text: "Nama", value: "nama" },
        { text: "No. Telepon", value: "noTelepon" },
        { text: "Actions", value: "actions", sortable: false },
      ],
    }),
  };
  </script>
  
  <style></style>