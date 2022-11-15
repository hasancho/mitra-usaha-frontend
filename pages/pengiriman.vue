<template>
  <div>
    <v-card class="mt-10 pb-5">
      <v-card class="ma-5" v-show="showForm">
        <v-card-title>{{ modeForm }} Data Pengiriman</v-card-title>
        <v-form ref="form" class="pa-5" v-model="valid" lazy-validation>
          <v-row>
            <v-col cols="4">
              <v-text-field
                v-model="tujuan"
                label="Tujuan"
                required
              ></v-text-field>
            </v-col>
            <v-col cols="4">
              <v-text-field
                v-model="tarif"
                label="Tarif"
                required
              ></v-text-field>
            </v-col>
          </v-row>
          <v-row>
            <v-col cols="4">
              <v-text-field
                v-model="biayaPokok"
                label="Biaya Pokok"
                required
              ></v-text-field>
            </v-col>
            <v-col cols="4">
              <v-text-field
                v-model="material"
                label="Material"
                required
              ></v-text-field>
            </v-col>
          </v-row>
          <v-btn color="success" class="mr-4" @click="savePengiriman">
            Submit
          </v-btn>
          <v-btn color="error" class="mr-4" @click="reset"> Reset Form </v-btn>
        </v-form>
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
            <v-toolbar-title>Data Pengiriman</v-toolbar-title>
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
          <v-icon small class="mr-2" @click="editPengiriman(item)">
            mdi-pencil
          </v-icon>
          <v-icon small @click="deletePengriman(item)"> mdi-delete </v-icon>
        </template>
      </v-data-table>
    </v-card>
  </div>
</template>

<script>
export default {
  data: () => ({
    tujuan: "",
    tarif: "",
    biayaPokok: 0,
    material: "",
    showForm: false,
    showTable: true,
    modeForm: "Tambah",
    pengiriman: [],
    headers: [
      {
        text: "Tujuan",
        align: "start",
        sortable: false,
        value: "tujuan",
      },
      { text: "Tarif", value: "tarif" },
      { text: "Biaya Pokok", value: "biaya_pokok" },
      { text: "Material", value: "material" },
      { text: "Actions", value: "actions", sortable: false },
    ],
  }),
  mounted() {
    this.getPengiriman();
  },
  methods: {
    formVisibilty(valueForm = "Tambah", valueShowForm, valueShowTable) {
      this.modeForm = valueForm;
      this.showForm = valueShowForm;
      this.showTable = valueShowTable;
    },
    async getPengiriman() {
      const getPengiriman = await this.$axios("/pengiriman");
      this.pengiriman = getPengiriman.data;
    },
    savePengiriman() {
      if (this.modeForm == "Tambah") {
        const result = this.$axios.post("/pengiriman", {
          tujuan: this.tujuan,
          tarif: this.tarif,
          biaya_pokok: this.biayaPokok,
          material: this.material,
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
            tujuan: this.tujuan,
            tarif: this.tarif,
            biaya_pokok: this.biayaPokok,
            material: this.material,
          })
          .then((result) => {
            clearAndRefreshForm(result);
          });
      }
    },
    editPengiriman(pengiriman) {
      this.showTable = false;
      this.modeForm = "Ubah";
      this.showForm = true;
      this.tujuan = pengiriman.tujuan;
      this.tarif = pengiriman.tarif;
      this.biayaPokok = pengiriman.biaya_pokok;
      this.material = pengiriman.material;
    },
    clearAndRefreshForm() {
      this.formVisibilty(false);
      this.tujuan = "";
      this.tarif = "";
      this.biayaPokok = 0;
      this.material = "";
      this.showTable = true;
      this.getPengiriman();
    },
    reset() {
      this.$refs.form.reset();
    },
  },
};
</script>
