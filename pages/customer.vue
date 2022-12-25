<template>
  <div>
    <v-spacer></v-spacer>
    <!-- <v-alert v-show="showAlert" dense text type="success">
      Berhasil tambah data
    </v-alert> -->
    <v-card class="mt-10 pb-5">
      <v-divider></v-divider>
      <v-card class="ma-5" v-show="showForm">
        <v-card-title>Data Customer</v-card-title>
        <v-form ref="form" class="pa-5" lazy-validation>
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
          <v-btn color="success" class="mr-4" @click="saveCustomer">
            Submit
          </v-btn>
          <v-btn color="error" class="mr-4" @click="reset"> Reset Form </v-btn>
        </v-form>
      </v-card>
      <v-data-table
        v-show="showTable"
        :headers="headers"
        :items="customer"
        sort-by="calories"
        class="elevation-1"
      >
        <template v-slot:top>
          <v-toolbar flat>
            <v-toolbar-title>Data Customer</v-toolbar-title>
            <v-spacer></v-spacer>
            <v-dialog max-width="500px">
              <template v-slot:activator="{}">
                <v-btn
                  color="primary"
                  dark
                  class="mb-2"
                  v-on:click="formVisibilty('Tambah', true, false)"
                >
                  New Item
                </v-btn>
              </template>
            </v-dialog>
          </v-toolbar>
        </template>

        <template v-slot:item.actions="{ item }">
          <v-icon small class="mr-2" @click="editCustomer(item)">
            mdi-pencil
          </v-icon>
          <v-icon small class="mr-2" @click="deleteCustomer(item)">
            mdi-delete
          </v-icon>
          <!-- <v-icon small class="mr-2" @click="viewCustomer(item)">
            mdi-eye
          </v-icon> -->
        </template>
      </v-data-table>
    </v-card>
  </div>
</template>

<script>
import CardCustomer from "../components/CardCustomer.vue";
export default {
  components: {
    CardCustomer,
  },
  data: () => ({
    id_customer: null,
    npwp: "",
    nama: "",
    alamat: "",
    noTelepon: null,
    customer: [],
    showForm: true,
    modeForm: "Tambah",
    showTable: true,
    showAlert: false,
    showForm: false,
    headers: [
      {
        text: "NPWP",
        align: "start",
        sortable: false,
        value: "npwp",
      },
      { text: "Nama", value: "nama" },
      { text: "Alamat", value: "alamat" },
      { text: "No. Telepon", value: "no_telepon" },
      { text: "Actions", value: "actions", sortable: false },
    ],
  }),
  watch: {
    dialog(val) {
      val || this.close();
    },
    dialogDelete(val) {
      val || this.closeDelete();
    },
  },
  mounted() {
    this.getCustomer();
  },
  methods: {
    formVisibilty(valueForm = "Tambah", valueShowForm, valueShowTable) {
      this.modeForm = valueForm;
      this.showForm = valueShowForm;
      this.showTable = valueShowTable;
    },

    async getCustomer() {
      const getCustomer = await this.$axios("/customer");
      this.customer = getCustomer.data;
      console.log(this.customer);
    },

    viewCustomer(customer) {
      this.showTable = false;
      this.modeForm = "Lihat";
      this.showForm = true;
      this.npwp = customer.npwp;
      this.nama = customer.nama;
      this.alamat = customer.alamat;
      this.noTelepon = customer.no_telepon;
      this.id_customer = customer.id_customer;
    },

    editCustomer(customer) {
      this.showTable = false;
      this.modeForm = "Ubah";
      this.showForm = true;
      this.npwp = customer.npwp;
      this.nama = customer.nama;
      this.alamat = customer.alamat;
      this.noTelepon = customer.no_telepon;
      this.id_customer = customer.id_customer;
    },

    saveCustomer() {
      if (this.modeForm == "Tambah") {
        const result = this.$axios.post("/customer", {
          npwp: this.npwp,
          nama: this.nama,
          alamat: this.alamat,
          no_telepon: this.noTelepon,
        });
        return result
          .then((result) => {
            this.clearAndRefreshForm(result);
            this.showAlert = true;
          })
          .catch((error) => {
            console.log(error);
          });
      } else if (this.modeForm === "Ubah") {
        this.$axios
          .put("/customer", {
            npwp: this.npwp,
            nama: this.nama,
            alamat: this.alamat,
            no_telepon: this.noTelepon,
            id_customer: this.id_customer,
          })
          .then((result) => {
            console.log(result.config.data);
            this.clearAndRefreshForm(result);
          });
      }
    },

    deleteCustomer(customer) {
      this.$axios
        .delete("/customer/" + customer.id_customer)
        .then((response) => {
          alert(response.data.message);
          this.getCustomer();
        });
    },

    clearAndRefreshForm(result) {
      alert(result.data.message);
      this.formVisibilty(false);
      this.npwp = "";
      this.nama = "";
      this.alamat = "";
      this.noTelepon = "";
      this.showTable = true;
      this.getCustomer();
    },

    reset() {
      this.$refs.form.reset();
    },
  },
};
</script>
