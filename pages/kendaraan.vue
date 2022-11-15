<template>
  <div>
    <v-card class="mt-10 pb-5">
      <v-card class="ma-5" v-show="showForm">
        <v-card-title>{{ modeForm }} Data Kendaraan</v-card-title>
        <v-form ref="form" class="pa-5" lazy-validation>
          <v-row>
            <v-col cols="4">
              <v-text-field
                v-model="noPol"
                label="No. Pol"
                required
              ></v-text-field>
            </v-col>
            <v-col cols="4">
              <v-text-field
                v-model="tahun"
                label="Tahun"
                required
              ></v-text-field>
            </v-col>
          </v-row>
          <v-row>
            <v-col cols="4">
              <v-text-field
                v-model="jenis"
                label="Jenis"
                required
              ></v-text-field>
            </v-col>
            <v-col cols="4">
              <v-text-field v-model="tipe" label="Tipe" required></v-text-field>
            </v-col>
          </v-row>
          <v-row>
            <v-col cols="4">
              <v-text-field
                v-model="nomor_rangka"
                label="Nomer Rangka"
                required
              ></v-text-field>
            </v-col>
            <v-col cols="4">
              <v-text-field
                v-model="nomer_mesin"
                label="Nomer Mesin"
                required
              ></v-text-field>
            </v-col>
          </v-row>
          <v-row>
            <v-col cols="4">
              <v-text-field
                v-model="value"
                label="Masa Berlaku STNK"
                dense
                hide-details
                class="pa-5"
              >
                <template v-slot:append-outer>
                  <date-picker v-model="value" />
                </template>
              </v-text-field>
            </v-col>
            <v-col cols="4">
              <v-text-field
                v-model="value"
                label="Masa Berlaku Pajak"
                dense
                hide-details
                class="pa-5"
              >
                <template v-slot:append-outer>
                  <date-picker v-model="value" />
                </template>
              </v-text-field>
            </v-col>
          </v-row>
          <v-row>
            <v-col cols="4">
              <v-text-field
                v-model="value"
                label="Masa Berlaku Kir"
                dense
                hide-details
                class="pa-5"
              >
                <template v-slot:append-outer>
                  <date-picker v-model="value" />
                </template>
              </v-text-field>
            </v-col>
          </v-row>
          <v-btn color="success" class="mr-4"> Submit </v-btn>
          <v-btn color="error" class="mr-4" @click="reset"> Reset Form </v-btn>
        </v-form>
      </v-card>
      <v-data-table
        v-show="showTable"
        :headers="headers"
        :items="kendaraan"
        sort-by="calories"
        class="elevation-1"
      >
        <template v-slot:top>
          <v-toolbar flat>
            <v-toolbar-title>Data Kendaraan</v-toolbar-title>
            <v-spacer></v-spacer>
            <v-dialog v-model="dialog" max-width="500px">
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
          <v-icon small class="mr-2" @click="editKendaraan(item)">
            mdi-pencil
          </v-icon>
          <v-icon small @click="deleteKendaraan(item)"> mdi-delete </v-icon>
        </template>
        <template v-slot:no-data>
          <v-btn color="primary" @click="initialize"> Reset </v-btn>
        </template>
      </v-data-table>
    </v-card>
  </div>
</template>

<script>
export default {
  data: () => ({
    noPol: "",
    tahun: "",
    jenis: "",
    tipe: "",
    nomor_rangka: "",
    nomor_mesin: "",
    masa_berlaku_stnk: "",
    masa_berlaku_pajak: "",
    masa_berlaku_kir: "",
    showForm: false,
    showTable: true,
    modeForm: "Tambah",
    kendaraan: [],
    headers: [
      {
        text: "No. Pol",
        align: "start",
        sortable: false,
        value: "no_pol",
      },
      { text: "Jenis", value: "jenis" },
      { text: "Tipe", value: "tipe" },
      { text: "Masa Berlaku STNK", value: "masa_berlaku_stnk" },
      { text: "Actions", value: "actions", sortable: false },
    ],
  }),
  mounted() {
    this.getKendaraan();
  },
  methods: {
    formVisibilty(valueForm = "Tambah", valueShowForm, valueShowTable) {
      this.modeForm = valueForm;
      this.showForm = valueShowForm;
      this.showTable = valueShowTable;
    },
    async getKendaraan() {
      const getKendaraan = await this.$axios("/kendaraan");
      this.kendaraan = getKendaraan.data;
    },
    saveKendaraan() {
      if (this.modeForm == "Tambah") {
        const result = this.$axios.post("/kendaraan", {
          no_pol: this.noPol,
          tahun: this.tahun,
          jenis: this.jenis,
          tipe: this.tipe,
          nomor_rangka: this.nomor_rangka,
          nomor_mesin: this.nomor_mesin,
          masa_berlaku_stnk: this.masa_berlaku_stnk,
          masa_berlaku_paja: this.masa_berlaku_pajak,
          masa_berlaku_kir: this.masa_berlaku_kir,
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
          .put("/kendaraan", {
            no_pol: this.noPol,
            tahun: this.tahun,
            jenis: this.jenis,
            tipe: this.tipe,
            nomor_rangka: this.nomor_rangka,
            nomor_mesin: this.nomor_mesin,
            masa_berlaku_stnk: this.masa_berlaku_stnk,
            masa_berlaku_paja: this.masa_berlaku_pajak,
            masa_berlaku_kir: this.masa_berlaku_kir,
          })
          .then((result) => {
            clearAndRefreshForm(result);
          });
      }
    },
    editKendaraan(kendaraan) {
      this.showTable = false;
      this.modeForm = "Ubah";
      this.showForm = true;
      this.noPol = kendaraan.no_pol;
      this.tahun = kendaraan.tahun;
      this.jenis = kendaraan.jenis;
      this.tipe = kendaraan.tipe;
      this.nomor_rangka = kendaraan.nomor_rangka;
      this.nomor_mesin = kendaraan.nomor_mesin;
      this.masa_berlaku_stnk = kendaraan.masa_berlaku_stnk;
      this.masa_berlaku_pajak = kendaraan.masa_berlaku_pajak;
      this.masa_berlaku_kir = kendaraan.masa_berlaku_kir;
      this.getKendaraan();
    },
    clearAndRefreshForm() {
      this.formVisibilty(false);
      this.noPol = "";
      this.tahun = 0;
      this.jenis = "";
      this.tipe = "";
      this.nomor_rangka = "";
      this.nomor_mesin = "";
      this.masa_berlaku_stnk = "";
      this.masa_berlaku_pajak = "";
      this.masa_berlaku_kir = "";
      this.getKendaraan();
    },
    reset() {
      this.$refs.form.reset();
    },
  },
};
</script>
