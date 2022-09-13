<template>
  <div>
    <v-card class="mt-10 pb-5">
      <v-card-title>Data Customer</v-card-title>
      <v-divider></v-divider>
      <v-card class="ma-5" v-show="showForm">
        <v-form ref="form" class="pa-5" v-model="valid" lazy-validation>
          <v-row>
            <v-col cols="4">
              <v-text-field v-model="npwp" label="NPWP" required></v-text-field>
            </v-col>
            <v-col cols="4">
              <v-text-field v-model="nama" label="Nama" required></v-text-field>
            </v-col>
          </v-row>
          <v-row>
            <v-col cols="4">
              <v-text-field
                v-model="alamat"
                label="Alamat"
                required
              ></v-text-field>
            </v-col>
            <v-col cols="4">
              <v-text-field
                v-model="noTelepon"
                label="No. Telepon"
                required
              ></v-text-field>
            </v-col>
          </v-row>
          <v-btn
            :disabled="!valid"
            color="success"
            class="mr-4"
            @click="validate"
          >
            Submit
          </v-btn>

          <v-btn color="error" class="mr-4" @click="reset"> Reset Form </v-btn>
        </v-form>
      </v-card>
      <v-card class="ma-5">
        <v-data-table
          :headers="headers"
          :items="data"
          sort-by="calories"
          class="elevation-1"
        >
          <template v-slot:top>
            <v-toolbar flat>
              <v-spacer></v-spacer>
              <v-dialog v-model="dialog" max-width="500px">
                <template v-slot:activator="{ on, attrs }">
                  <v-btn
                    color="primary"
                    dark
                    class="mb-2"
                    v-bind="attrs"
                    v-on="on"
                    v-on:click="formVisibilty('add', true)"
                  >
                    Tambah
                  </v-btn>
                </template>
              </v-dialog>
            </v-toolbar>
          </template>
          <template v-slot:item.actions :item="{ item }">
            <v-icon small class="mr-2" @click="editItem(item)">
              mdi-pencil
            </v-icon>
            <v-icon small @click="deleteItem(item)"> mdi-delete </v-icon>
          </template>
          <template v-slot:no-data>
            <v-btn color="primary" @click="initialize"> Reset </v-btn>
          </template>
        </v-data-table>
      </v-card>
    </v-card>
  </div>
</template>

<script>
export default {
  data: () => ({
    npwp: "",
    nama: "",
    alamat: "",
    noTelepon: null,
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
    data: [],
    showForm: false,
    modeForm: "add",
  }),
  computed: {
    formTitle() {
      return this.editedIndex === -1 ? "New Item" : "Edit Item";
    },
  },

  watch: {
    created() {
      this.initialize();
    },
    methods: {
      formVisibilty(valueForm = "add", value) {
        (this.modeForm = valueForm), (this.showForm = value);
      },
      reset() {
        this.$refs.form.reset();
      },
      initialize() {
        this.data = [
          {
            no: 1,
            nama: "Ahmad Fauzi",
            npwp: 1598942354,
            noTelepon: "08377687824",
          },
          {
            no: 2,
            nama: "Anasia Rahma",
            npwp: 1598942355,
            noTelepon: "08377687123",
          },
        ];
      },
    },
  },
};
</script>
