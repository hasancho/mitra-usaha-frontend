<template>
  <div>
    <v-card class="mt-10 pb-5">
      <v-card class="ma-5" v-show="showForm">
        <v-card-title>{{ modeForm }} Data Kendaraan</v-card-title>
        <v-form ref="form" class="pa-5" lazy-validation>
          <v-row>
            <v-col cols="4">
              <v-text-field
                v-model="kode_jo"
                label="Kode JO"
                required
              ></v-text-field>
            </v-col>
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
                v-model="nomor_mesin"
                label="Nomer Mesin"
                required
              ></v-text-field>
            </v-col>
          </v-row>
          <v-row>
            <v-col cols="4">
              <v-menu
                ref="menu1"
                v-model="menu1"
                :close-on-content-click="false"
                :return-value.sync="masa_berlaku_stnk"
                transition="scale-transition"
                offset-y
                min-width="auto"
              >
                <template v-slot:activator="{ on, attrs }">
                  <v-text-field
                    v-model="masa_berlaku_stnk"
                    label="Masa Berlaku STNK"
                    prepend-icon="mdi-calendar"
                    readonly
                    v-bind="attrs"
                    v-on="on"
                  ></v-text-field>
                </template>
                <v-date-picker v-model="masa_berlaku_stnk" no-title scrollable>
                  <v-spacer></v-spacer>
                  <v-btn text color="primary" @click="menu1 = false">
                    Cancel
                  </v-btn>
                  <v-btn
                    text
                    color="primary"
                    @click="$refs.menu1.save(masa_berlaku_stnk)"
                  >
                    OK
                  </v-btn>
                </v-date-picker>
              </v-menu>
            </v-col>
            <v-col cols="4">
              <v-menu
                ref="menu2"
                v-model="menu2"
                :close-on-content-click="false"
                :return-value.sync="masa_berlaku_pajak"
                transition="scale-transition"
                offset-y
                min-width="auto"
              >
                <template v-slot:activator="{ on, attrs }">
                  <v-text-field
                    v-model="masa_berlaku_pajak"
                    label="Masa Berlaku Pajak"
                    prepend-icon="mdi-calendar"
                    readonly
                    v-bind="attrs"
                    v-on="on"
                  ></v-text-field>
                </template>
                <v-date-picker v-model="masa_berlaku_pajak" no-title scrollable>
                  <v-spacer></v-spacer>
                  <v-btn text color="primary" @click="menu2 = false">
                    Cancel
                  </v-btn>
                  <v-btn
                    text
                    color="primary"
                    @click="$refs.menu2.save(masa_berlaku_pajak)"
                  >
                    OK
                  </v-btn>
                </v-date-picker>
              </v-menu>
            </v-col>
          </v-row>
          <v-row>
            <v-col cols="4">
              <v-menu
                ref="menu3"
                v-model="menu3"
                :close-on-content-click="false"
                :return-value.sync="masa_berlaku_kir"
                transition="scale-transition"
                offset-y
                min-width="auto"
              >
                <template v-slot:activator="{ on, attrs }">
                  <v-text-field
                    v-model="masa_berlaku_kir"
                    label="Masa Berlaku Kir"
                    prepend-icon="mdi-calendar"
                    readonly
                    v-bind="attrs"
                    v-on="on"
                  ></v-text-field>
                </template>
                <v-date-picker v-model="masa_berlaku_kir" no-title scrollable>
                  <v-spacer></v-spacer>
                  <v-btn text color="primary" @click="menu3 = false">
                    Cancel
                  </v-btn>
                  <v-btn
                    text
                    color="primary"
                    @click="$refs.menu3.save(masa_berlaku_kir)"
                  >
                    OK
                  </v-btn>
                </v-date-picker>
              </v-menu>
            </v-col>
          </v-row>
          <v-btn color="success" class="mr-4" v-on:click="saveKendaraan">
            Submit
          </v-btn>
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
          <v-icon small class="mr-2" @click="editKendaraan(item)">
            mdi-pencil
          </v-icon>
          <v-icon small @click="deleteKendaraan(item)"> mdi-delete </v-icon>
        </template>
      </v-data-table>
    </v-card>
  </div>
</template>

<script>
export default {
  data: () => ({
    id_kendaraan: null,
    kode_jo: "",
    noPol: "",
    tahun: "",
    jenis: null,
    tipe: "",
    nomor_rangka: "",
    nomor_mesin: "",
    masa_berlaku_stnk: new Date(
      Date.now() - new Date().getTimezoneOffset() * 60000
    )
      .toISOString()
      .substr(0, 10),
    masa_berlaku_pajak: new Date(
      Date.now() - new Date().getTimezoneOffset() * 60000
    )
      .toISOString()
      .substr(0, 10),
    masa_berlaku_kir: new Date(
      Date.now() - new Date().getTimezoneOffset() * 60000
    )
      .toISOString()
      .substr(0, 10),
    modal: false,
    menu1: false,
    menu2: false,
    menu3: false,
    showForm: false,
    showTable: true,
    modeForm: "Tambah",
    kendaraan: [],
    headers: [
      {
        text: "Kode JO",
        align: "start",
        value: "kode_jo",
      },
      { text: "No. Pol", value: "no_pol" },
      { text: "Jenis", value: "jenis" },
      { text: "Tipe", value: "tipe" },
      { text: "Actions", value: "actions", sortable: false },
    ],

    // masa_berlaku_stnk: new Date(
    //   Date.now() - new Date().getTimezoneOffset() * 60000
    // )
    //   .toISOString()
    //   .substr(0, 10),
    // masa_berlaku_pajak: new Date(
    //   Date.now() - new Date().getTimezoneOffset() * 60000
    // )
    //   .toISOString()
    //   .substr(0, 10),
    // masa_berlaku_kir: new Date(
    //   Date.now() - new Date().getTimezoneOffset() * 60000
    // )
    //   .toISOString()
    //   .substr(0, 10),
    // dateFormatted1: vm.formatDate(
    //   new Date(Date.now() - new Date().getTimezoneOffset() * 60000)
    //     .toISOString()
    //     .substr(0, 10)
    // ),
    // dateFormatted2: vm.formatDate(
    //   new Date(Date.now() - new Date().getTimezoneOffset() * 60000)
    //     .toISOString()
    //     .substr(0, 10)
    // ),
    // dateFormatted3: vm.formatDate(
    //   new Date(Date.now() - new Date().getTimezoneOffset() * 60000)
    //     .toISOString()
    //     .substr(0, 10)
    // ),
  }),
  watch: {
    // masa_berlaku_stnk(val) {
    //   this.dateFormatted1 = this.formatDate(this.masa_berlaku_stnk);
    // },
    // masa_berlaku_pajak(val) {
    //   this.dateFormatted2 = this.formatDate(this.masa_berlaku_pajak);
    // },
    // masa_berlaku_kir(val) {
    //   this.dateFormatted3 = this.formatDate(this.masa_berlaku_kir);
    // },
  },
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
          kode_jo: this.kode_jo,
          no_pol: this.noPol,
          tahun: this.tahun,
          jenis: this.jenis,
          tipe: this.tipe,
          nomor_rangka: this.nomor_rangka,
          nomor_mesin: this.nomor_mesin,
          masa_berlaku_stnk: this.masa_berlaku_stnk,
          masa_berlaku_pajak: this.masa_berlaku_pajak,
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
            kode_jo: this.kode_jo,
            no_pol: this.noPol,
            tahun: this.tahun,
            jenis: this.jenis,
            tipe: this.tipe,
            nomor_rangka: this.nomor_rangka,
            nomor_mesin: this.nomor_mesin,
            masa_berlaku_stnk: this.masa_berlaku_stnk,
            masa_berlaku_pajak: this.masa_berlaku_pajak,
            masa_berlaku_kir: this.masa_berlaku_kir,
            id_kendaraan: this.id_kendaraan,
          })
          .then((result) => {
            this.clearAndRefreshForm(result);
          });
      }
    },
    editKendaraan(kendaraan) {
      this.showTable = false;
      this.modeForm = "Ubah";
      this.showForm = true;
      this.kode_jo = kendaraan.kode_jo;
      this.noPol = kendaraan.no_pol;
      this.tahun = kendaraan.tahun;
      this.jenis = kendaraan.jenis;
      this.tipe = kendaraan.tipe;
      this.nomor_rangka = kendaraan.nomor_rangka;
      this.nomor_mesin = kendaraan.nomor_mesin;
      this.masa_berlaku_stnk = kendaraan.masa_berlaku_stnk;
      this.masa_berlaku_pajak = kendaraan.masa_berlaku_pajak;
      this.masa_berlaku_kir = kendaraan.masa_berlaku_kir;
      this.id_kendaraan = kendaraan.id_kendaraan;
    },
    clearAndRefreshForm(result) {
      alert(result.data.message);
      this.formVisibilty(false);
      this.kode_jo = "";
      this.noPol = "";
      this.tahun = null;
      this.jenis = "";
      this.tipe = "";
      this.nomor_rangka = "";
      this.nomor_mesin = "";
      this.masa_berlaku_stnk = "";
      this.masa_berlaku_pajak = "";
      this.masa_berlaku_kir = "";
      this.showTable = true;
      this.getKendaraan();
    },
    deleteKendaraan(kendaraan) {
      this.$axios
        .delete("/kendaraan/" + kendaraan.id_kendaraan)
        .then((response) => {
          alert(response.data.message);
          this.getKendaraan();
        });
    },
    formatDate(date) {
      if (!date) return null;

      const [year, month, day] = date.split("-");
      return `${month}/${day}/${year}`;
    },
    parseDate(date) {
      if (!date) return null;

      const [month, day, year] = date.split("/");
      return `${year}-${month.padStart(2, "0")}-${day.padStart(2, "0")}`;
    },
    reset() {
      this.$refs.form.reset();
    },
  },
};
</script>
