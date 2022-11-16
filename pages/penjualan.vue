<template>
  <div>
    <v-card class="mt-10 pb-5">
      <v-card class="ma-5" v-show="showForm">
        <v-card-title>{{ modeForm }} Data Penjualan</v-card-title>
        <v-form ref="form" class="pa-5" v-model="valid" lazy-validation>
          <v-row>
            <v-col cols="4">
              <v-select
                v-model="customer"
                :items="listNamaCustomer"
                label="Customer"
                required
              ></v-select>
            </v-col>
            <v-col cols="4">
              <v-select
                v-model="pengiriman"
                :items="listIdPengiriman"
                :rules="[(v) => !!v || 'Item is required']"
                label="Pengiriman"
                required
              ></v-select>
            </v-col>
          </v-row>
          <v-row>
            <v-col cols="4">
              <v-select
                v-model="kendaraan"
                :items="listNoPolKendaraan"
                label="Kendaraan"
                required
              ></v-select>
            </v-col>
            <v-col cols="4">
              <v-select
                v-model="karyawan"
                :items="listNamaKaryawan"
                label="Karyawan"
                required
              ></v-select>
            </v-col>
          </v-row>
          <v-row>
            <v-col cols="4">
              <v-text-field
                v-model="no_do"
                label="No. Do"
                required
              ></v-text-field>
            </v-col>
            <!-- <v-col cols="4">
              <v-text-field
                v-model="value"
                label="Tanggal"
                dense
                hide-details
                class="pa-5"
              >
                <template v-slot:append-outer>
                  <date-picker v-model="value" />
                </template>
              </v-text-field>
            </v-col> -->
          </v-row>
          <v-row>
            <v-col cols="4">
              <v-text-field
                v-model="quantity"
                :rules="nameRules"
                label="Quantity"
                required
              ></v-text-field>
            </v-col>
            <v-col cols="4">
              <v-radio-group v-model="pembayaran" label="Pembayaran">
                <v-radio
                  v-for="(pilihan, i) in status_pembayaran"
                  :key="i"
                  :label="pilihan"
                  :value="pilihan"
                  :pilihan="pilihan"
                ></v-radio>
              </v-radio-group>
            </v-col>
          </v-row>
          <v-row>
            <v-col cols="4">
              <v-text-field
                v-model="total_biaya"
                label="Total Biaya"
                required
              ></v-text-field>
            </v-col>
          </v-row>
          <v-btn color="success" class="mr-4" v-on:click="savePenjualan">
            Submit
          </v-btn>
          <v-btn color="error" class="mr-4" @click="reset"> Reset Form </v-btn>
        </v-form>
      </v-card>
      <v-data-table
        v-show="showTable"
        :headers="headers"
        :items="penjualan"
        sort-by="calories"
        class="elevation-1"
      >
        <template v-slot:top>
          <v-toolbar flat>
            <v-toolbar-title>Data Penjualan</v-toolbar-title>
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
          <v-icon small class="mr-2" @click="editPenjualan(item)">
            mdi-pencil
          </v-icon>
          <v-icon small @click="deletePenjualan(item)"> mdi-delete </v-icon>
        </template>
      </v-data-table>
    </v-card>
  </div>
</template>

<script>
export default {
  data: () => ({
    id: null,
    customer: "",
    listCustomer: [],
    listNamaCustomer: [],
    pengiriman: "",
    listPengiriman: [],
    listIdPengiriman: [],
    kendaraan: "",
    listKendaraan: [],
    listNoPolKendaraan: [],
    karyawan: "",
    listKaryawan: [],
    listNamaKaryawan: [],
    customer: "",
    pengiriman: "",
    kendaraan: "",
    karyawan: "",
    no_do: "",
    quantity: 0,
    pembayaran: "",
    total_biaya: 0,
    penjualan: [],
    showForm: false,
    showTable: true,
    modeForm: "Tambah",
    value: null,
    radioGroup: "",
    select: null,
    status_pembayaran: ["lunas", "belum lunas"],
    checkbox: false,
    headers: [
      {
        text: "No. Do",
        align: "start",
        sortable: false,
        value: "no_do",
      },
      { text: "Customer", value: "id_customer" },
      { text: "Pengiriman", value: "id_pengiriman" },
      { text: "Kendaraan", value: "id_kendaraan" },
      { text: "karyawan", value: "id_karyawan" },
      { text: "Actions", value: "actions", sortable: false },
    ],
  }),
  mounted() {
    this.getPenjualan();
    this.getCustomer();
    this.getPengiriman();
    this.getKendaraan();
    this.getKaryawan();
  },
  methods: {
    formVisibilty(valueForm = "Tambah", valueShowForm, valueShowTable) {
      this.modeForm = valueForm;
      this.showForm = valueShowForm;
      this.showTable = valueShowTable;
    },
    async getPenjualan() {
      const getPenjualan = await this.$axios("/penjualan");
      this.penjualan = getPenjualan.data;
    },
    async getCustomer() {
      const getCustomer = await this.$axios("/customer");
      this.listCustomer = getCustomer.data;
      for (let i = 0; i < this.listCustomer.length; i++) {
        this.listNamaCustomer.push(this.listCustomer[i].id);
      }
    },
    async getPengiriman() {
      const getPengiriman = await this.$axios("/pengiriman");
      this.listPengiriman = getPengiriman.data;
      for (let i = 0; i < this.listPengiriman.length; i++) {
        this.listIdPengiriman.push(this.listPengiriman[i].id);
      }
    },
    async getKendaraan() {
      const getKendaraan = await this.$axios("/kendaraan");
      this.listKendaraan = getKendaraan.data;
      for (let i = 0; i < this.listKendaraan.length; i++) {
        this.listNoPolKendaraan.push(this.listKendaraan[i].id);
      }
    },
    async getKaryawan() {
      const getKaryawan = await this.$axios("/karyawan");
      this.listKaryawan = getKaryawan.data;
      for (let i = 0; i < this.listKaryawan.length; i++) {
        this.listNamaKaryawan.push(this.listKaryawan[i].id);
      }
    },
    // calculateTotalBiaya() {
    //   this.total_biaya = this.tarif * this.quantity;
    // },
    savePenjualan() {
      if (this.modeForm === "Tambah") {
        const result = this.$axios.post("/penjualan", {
          id_customer: this.customer,
          id_pengiriman: this.pengiriman,
          id_kendaraan: this.kendaraan,
          id_karyawan: this.karyawan,
          no_do: this.no_do,
          quantity: this.quantity,
          pembayaran: this.pembayaran,
          total_biaya: this.total_biaya,
        });
        return result
          .then((result) => {
            this.clearAndRefreshForm(result);
          })
          .catch((error) => {
            console.log(error);
          });
      } else if (this.modeForm === "Ubah") {
        this.$axios
          .put("/penjualan", {
            id_customer: this.customer,
            id_pengiriman: this.pengiriman,
            id_kendaraan: this.kendaraan,
            id_karyawan: this.karyawan,
            no_do: this.no_do,
            quantity: this.quantity,
            pembayaran: this.pembayaran,
            total_biaya: this.total_biaya,
            id: this.id,
          })
          .then((result) => {
            this.clearAndRefreshForm(result);
          });
      }
    },
    editPenjualan(penjualan) {
      this.showTable = false;
      this.modeForm = "Ubah";
      this.showForm = true;
      this.customer = penjualan.id_customer;
      this.pengiriman = penjualan.id_pengiriman;
      this.kendaraan = penjualan.id_kendaraan;
      this.karyawan = penjualan.id_karyawan;
      this.no_do = penjualan.no_do;
      this.quantity = penjualan.quantity;
      this.pembayaran = penjualan.pembayaran;
      this.total_biaya = penjualan.total_biaya;
      this.id = penjualan.id;
    },
    deletePenjualan(penjualan) {
      this.$axios.delete("/penjualan/" + penjualan.id).then((response) => {
        alert(response.data.message);
        this.penjualan();
      });
    },
    clearAndRefreshForm(result) {
      alert(result.data.message);
      this.formVisibilty(false);
      this.customer = 0;
      this.pengiriman = 0;
      this.kendaraan = 0;
      this.karyawan = 0;
      this.no_do = "";
      this.quantity = 0;
      this.pembayaran = "";
      this.total_pembayaran = 0;
      this.showTable = true;
      this.getPenjualan();
    },
    reset() {
      this.$refs.form.reset();
    },
  },
};
</script>
