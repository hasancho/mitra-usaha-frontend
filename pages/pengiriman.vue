<template>
  <div>
    <v-card class="mt-10 pb-5">
      <v-divider></v-divider>
      <v-card class="ma-5" v-show="showForm">
        <v-card-title>{{ modeForm }} DATA TUJUAN PENGIRIMAN</v-card-title>
        <div v-show="modeForm === 'UBAH' || modeForm === 'TAMBAH'">
          <v-form ref="form" class="pa-5" lazy-validation>
            <v-row>
              <v-col cols="4">
                <v-text-field
                  v-model="kode_tujuan"
                  label="KODE TUJUAN"
                  required
                ></v-text-field>
              </v-col>
              <v-col cols="4">
                <v-text-field
                  v-model="tujuan"
                  label="TUJUAN"
                  required
                ></v-text-field>
              </v-col>
              <v-col cols="4">
                <v-text-field
                  v-model="tarif"
                  label="TARIF"
                  required
                ></v-text-field>
              </v-col>
            </v-row>
            <v-row>
              <v-col cols="4">
                <v-text-field
                  v-model="biayaPokok"
                  label="BIAYA POKOK"
                  required
                ></v-text-field>
              </v-col>
              <v-col cols="4">
                <v-text-field
                  v-model="komisi"
                  label="KOMISI"
                  required
                ></v-text-field>
              </v-col>
            </v-row>
            <v-btn color="success" class="mr-4" @click="savePengiriman">
              Submit
            </v-btn>
            <v-btn color="error" class="mr-4" @click="reset">
              Reset Form
            </v-btn>
          </v-form>
        </div>
        <div class="pa-5" v-if="modeForm === 'LIHAT'">
          <p>Kode Tujuan: {{ kode_tujuan }}</p>
          <p>Tujuan: {{ tujuan }}</p>
          <p>Tarif: {{ tarif }}</p>
          <p>Biaya Pokok: {{ biayaPokok }}</p>
          <p>Komisi: {{ komisi }}</p>
          <v-btn color="primary" v-on:click="clearAndRefreshForm">
            <router-link
              to="/pengiriman"
              style="color: white; text-decoration: none"
              >Back</router-link
            >
          </v-btn>
        </div>
      </v-card>
      <v-data-table
        v-show="showTable"
        :headers="headers"
        :items="pengiriman"
        sort-by="calories"
        class="elevation-1"
      >
        <template v-slot:top>
          <v-toolbar flat>
            <v-toolbar-title>DATA TUJUAN PENGIRIMAN</v-toolbar-title>
            <v-spacer></v-spacer>
            <v-dialog vmax-width="500px">
              <template v-slot:activator="{}">
                <v-btn
                  color="primary"
                  dark
                  class="mb-2"
                  v-on:click="formVisibilty('TAMBAH', true, false)"
                >
                  New Item
                </v-btn>
              </template>
            </v-dialog>
          </v-toolbar>
        </template>

        <template v-slot:item.actions="{ item }">
          <div
            v-if="
              loggedInUser.data.role === 'admin' ||
              loggedInUser.data.role === 'superadmin'
            "
            style="float: left"
          >
            <v-icon small class="mr-2" @click="editPengiriman(item)">
              mdi-pencil
            </v-icon>
            <v-icon small class="mr-3" @click="deletePengiriman(item)">
              mdi-delete
            </v-icon>
          </div>
          <v-icon small class="mr-2" @click="viewPengiriman(item)">
            mdi-eye
          </v-icon>
        </template>
      </v-data-table>
    </v-card>
  </div>
</template>

<script>
import { mapGetters } from "vuex";
export default {
  data: () => ({
    id_pengiriman: null,
    kode_tujuan: "",
    tujuan: "",
    tarif: "",
    biayaPokok: 0,
    material: "",
    komisi: null,
    showForm: false,
    showTable: true,
    modeForm: "TAMBAH",
    pengiriman: [],
    headers: [
      {
        text: "KODE TUJAN",
        align: "start",
        value: "kode_tujuan",
      },
      {
        text: "TUJUAN",
        value: "tujuan",
      },
      { text: "TARIF", value: "tarif" },
      { text: "BIAYA POKOK", value: "biaya_pokok" },
      { text: "KOMISI", value: "komisi" },
      { text: "Actions", value: "actions", sortable: false },
    ],
  }),
  computed: {
    ...mapGetters(["isAuthenticated", "loggedInUser"]),
  },
  mounted() {
    this.getPengiriman();
  },
  methods: {
    formVisibilty(valueForm = "TAMBAH", valueShowForm, valueShowTable) {
      this.modeForm = valueForm;
      this.showForm = valueShowForm;
      this.showTable = valueShowTable;
    },
    async getPengiriman() {
      const getPengiriman = await this.$axios("/pengiriman");
      this.pengiriman = getPengiriman.data;
    },
    savePengiriman() {
      if (this.modeForm == "TAMBAH") {
        const result = this.$axios.post("/pengiriman", {
          kode_tujuan: this.kode_tujuan,
          tujuan: this.tujuan,
          tarif: this.tarif,
          biaya_pokok: this.biayaPokok,
          komisi: this.komisi,
        });
        return result
          .then((result) => {
            alert(result.data.message);
            this.clearAndRefreshForm(result);
          })
          .catch((error) => {
            console.log(error);
          });
      } else if (this.modeForm === "UBAH") {
        this.$axios
          .put("/pengiriman", {
            kode_tujuan: this.kode_tujuan,
            tujuan: this.tujuan,
            tarif: this.tarif,
            biaya_pokok: this.biayaPokok,
            komisi: this.komisi,
            id_pengiriman: this.id_pengiriman,
          })
          .then((result) => {
            alert(result.data.message);
            this.clearAndRefreshForm(result);
          });
      }
    },
    viewPengiriman(pengiriman) {
      this.showTable = false;
      this.modeForm = "LIHAT";
      this.showForm = true;
      this.kode_tujuan = pengiriman.kode_tujuan;
      this.tujuan = pengiriman.tujuan;
      this.tarif = pengiriman.tarif;
      this.biayaPokok = pengiriman.biaya_pokok;
      this.komisi = pengiriman.komisi;
      this.id_pengiriman = pengiriman.id_pengiriman;
    },
    editPengiriman(pengiriman) {
      this.showTable = false;
      this.modeForm = "UBAH";
      this.showForm = true;
      this.kode_tujuan = pengiriman.kode_tujuan;
      this.tujuan = pengiriman.tujuan;
      this.tarif = pengiriman.tarif;
      this.biayaPokok = pengiriman.biaya_pokok;
      this.komisi = pengiriman.komisi;
      this.id_pengiriman = pengiriman.id_pengiriman;
    },
    deletePengiriman(pengiriman) {
      this.$axios
        .delete("/pengiriman/" + pengiriman.id_pengiriman)
        .then((response) => {
          alert(response.data.message);
          this.getPengiriman();
        });
    },
    clearAndRefreshForm(result) {
      this.formVisibilty(false);
      this.kode_tujuan = "";
      this.tujuan = "";
      this.tarif = "";
      this.biayaPokok = 0;
      this.komisi = null;
      this.showTable = true;
      this.getPengiriman();
    },
    reset() {
      this.$refs.form.reset();
    },
  },
};
</script>
