<template>
  <div>
    <v-card class="mt-10 pb-5">
      <v-card class="ma-5" v-show="showForm">
        <v-card-title>{{ modeForm }} Data Pengeluaran Kas</v-card-title>
        <v-form ref="form" class="pa-5" lazy-validation>
          <v-row>
            <v-col cols="4">
              <v-menu
                ref="menu1"
                v-model="menu1"
                :close-on-content-click="false"
                transition="scale-transition"
                offset-y
                max-width="290px"
                min-width="auto"
              >
                <template v-slot:activator="{ on, attrs }">
                  <v-text-field
                    v-model="dateFormatted"
                    label="Tanggal"
                    persistent-hint
                    prepend-icon="mdi-calendar"
                    v-bind="attrs"
                    @blur="date = parseDate(dateFormatted)"
                    v-on="on"
                  ></v-text-field>
                </template>
                <v-date-picker
                  v-model="tanggal"
                  no-title
                  @input="menu1 = false"
                ></v-date-picker>
              </v-menu>
            </v-col>
            <v-col cols="4">
              <v-text-field
                v-model="keterangan"
                label="Keterangan"
                required
              ></v-text-field>
            </v-col>
          </v-row>
          <v-row>
            <v-col cols="4">
              <v-text-field
                v-model="total_pengeluaran"
                label="Total"
                prefix="Rp."
                required
              ></v-text-field>
            </v-col>
            <!-- <v-col cols="4">
              <v-text-field v-model="akun" label="Akun" required></v-text-field>
            </v-col> -->
          </v-row>
          <v-btn color="success" class="mr-4" @click="savePengeluaranKas">
            Submit
          </v-btn>
          <v-btn color="error" class="mr-4" @click="reset"> Reset Form </v-btn>
        </v-form>
      </v-card>
      <v-data-table
        v-show="showTable"
        :headers="headers"
        :items="pengeluaran_kas"
        sort-by="calories"
        class="elevation-1"
      >
        <template v-slot:top>
          <v-toolbar flat>
            <v-toolbar-title>Data Pengeluaran Kas</v-toolbar-title>
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
          <v-icon small class="mr-2" @click="editPengeluaranKas(item)">
            mdi-pencil
          </v-icon>
          <v-icon small @click="deletePengeluaranKas(item)">
            mdi-delete
          </v-icon>
        </template>
      </v-data-table>
    </v-card>
  </div>
</template>

<script>
export default {
  data: (value) => ({
    id: null,
    tanggal: "",
    keterangan: "",
    total_pengeluaran: "",
    pengeluaran_kas: [],
    akun: "",
    showForm: false,
    showTable: true,
    modeForm: "Tambah",
    pilihan_status_karyawan: ["tetap", "tidak tetap"],
    tanggal: new Date(Date.now() - new Date().getTimezoneOffset() * 60000)
      .toISOString()
      .substr(0, 10),
    dateFormatted: value.formatDate(
      new Date(Date.now() - new Date().getTimezoneOffset() * 60000)
        .toISOString()
        .substr(0, 10)
    ),
    menu1: false,
    headers: [
      {
        text: "Tanggal",
        align: "start",
        sortable: false,
        value: "tanggal",
      },
      { text: "Keterangan", value: "keterangan" },
      { text: "Total Pengeluaran", value: "total_pengeluaran_kas" },
      { text: "Actions", value: "actions", sortable: false },
    ],
  }),
  watch: {
    tanggal(val) {
      this.dateFormatted = this.formatDate(this.tanggal);
    },
  },
  mounted() {
    this.getPengeluaranKas();
  },
  methods: {
    formVisibilty(valueForm = "Tambah", valueShowForm, valueShowTable) {
      this.modeForm = valueForm;
      this.showForm = valueShowForm;
      this.showTable = valueShowTable;
    },
    async getPengeluaranKas() {
      const getPengeluaranKas = await this.$axios("/pengeluaran_kas");
      this.pengeluaran_kas = getPengeluaranKas.data;
    },
    savePengeluaranKas() {
      if (this.modeForm == "Tambah") {
        const result = this.$axios.post("/pengeluaran_kas", {
          tanggal: this.tanggal,
          keterangan: this.keterangan,
          total_pengeluaran_kas: this.total_pengeluaran,
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
          .put("/pengeluaran_kas", {
            tanggal: this.tanggal,
            keterangan: this.keterangan,
            total_pengeluaran_kas: this.total_pengeluaran,
            id: this.id,
          })
          .then((result) => {
            this.clearAndRefreshForm(result);
          });
      }
    },
    editPengeluaranKas(pengeluaranKas) {
      this.showTable = false;
      this.modeForm = "Ubah";
      this.showForm = true;
      this.tanggal = pengeluaranKas.tanggal;
      this.keterangan = pengeluaranKas.keterangan;
      this.total_pengeluaran = pengeluaranKas.total_pengeluaran_kas;
      this.id = pengeluaranKas.id;
    },
    clearAndRefreshForm(result) {
      alert(result.data.message);
      this.formVisibilty(false);
      this.tanggal = "";
      this.keterangan = "";
      this.total_Pengeluaran = 0;
      this.showTable = true;
      this.getPengeluaranKas();
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
