<template>
  <div>
    <v-card class="mt-10 pb-5">
      <v-card class="ma-5" v-show="showForm">
        <v-card-title>{{ modeForm }} Data Karyawan</v-card-title>
        <v-form ref="form" class="pa-5" lazy-validation>
          <v-row>
            <v-col cols="4">
              <v-text-field v-model="nip" label="NIP" required></v-text-field>
            </v-col>
            <v-col cols="4">
              <v-text-field v-model="nik" label="NIK" required></v-text-field>
            </v-col>
          </v-row>
          <v-row>
            <v-col cols="4">
              <v-text-field v-model="nama" label="Nama" required></v-text-field>
            </v-col>
            <v-col cols="4">
              <v-text-field
                v-model="alamat"
                label="Alamat"
                required
              ></v-text-field>
            </v-col>
          </v-row>
          <v-row>
            <v-col cols="4">
              <v-text-field
                v-model="jabatan"
                label="Jabatan"
                required
              ></v-text-field>
            </v-col>
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
                    label="Tanggal Masuk"
                    persistent-hint
                    prepend-icon="mdi-calendar"
                    v-bind="attrs"
                    @blur="date = parseDate(dateFormatted)"
                    v-on="on"
                  ></v-text-field>
                </template>
                <v-date-picker
                  v-model="tanggal_masuk"
                  no-title
                  @input="menu1 = false"
                ></v-date-picker>
              </v-menu>
            </v-col>
          </v-row>
          <v-row>
            <v-col cols="3">
              <v-radio-group v-model="status_karyawan" label="Status Karyawan">
                <v-radio
                  v-for="(pilihan, i) in pilihan_status_karyawan"
                  :key="i"
                  :label="pilihan"
                  :value="pilihan"
                  :pilihan="pilihan"
                ></v-radio>
              </v-radio-group>
            </v-col>
            <v-col cols="4">
              <v-radio-group
                v-model="status_pernikahan"
                label="Status Pernikahan"
              >
                <v-radio
                  v-for="(pilihan, i) in pilihan_status_pernikahan"
                  :key="i"
                  :label="pilihan"
                  :value="pilihan"
                  :pilihan="pilihan"
                ></v-radio>
              </v-radio-group>
            </v-col>
            <v-col cols="4">
              <v-text-field v-model="gaji" label="Gaji" required></v-text-field>
            </v-col>
          </v-row>
          <v-btn color="success" class="mr-4" @click="saveKaryawan">
            Submit
          </v-btn>

          <v-btn color="error" class="mr-4" @click="reset"> Reset Form </v-btn>
        </v-form>
      </v-card>
    </v-card>
    <v-data-table
      v-show="showTable"
      :headers="headers"
      :items="karyawan"
      sort-by="calories"
      class="elevation-1"
    >
      <template v-slot:top>
        <v-toolbar flat>
          <v-toolbar-title>Data Karyawan</v-toolbar-title>
          <v-spacer></v-spacer>
          <v-dialog max-width="500px">
            <template v-slot:activator="{}">
              <v-btn
                color="primary"
                dark
                class="mb-2"
                v-on:click="formVisibility('Tambah', true, false)"
              >
                New Item
              </v-btn>
            </template>
          </v-dialog>
        </v-toolbar>
      </template>
      <template v-slot:item.actions="{ item }">
        <v-icon small class="mr-2" @click="editKaryawan(item)">
          mdi-pencil
        </v-icon>
        <v-icon small @click="deleteKaryawan(item)"> mdi-delete </v-icon>
      </template>
    </v-data-table>
  </div>
</template>

<script>
export default {
  data: (vm) => ({
    id: null,
    nip: "",
    nik: "",
    nama: "",
    alamat: "",
    status_karyawan: "",
    pilihan_status_karyawan: ["tetap", "tidak tetap"],
    status_pernikahan: "",
    pilihan_status_pernikahan: ["sudah", "belum"],
    jabatan: "",
    tanggal_masuk: new Date(Date.now() - new Date().getTimezoneOffset() * 60000)
      .toISOString()
      .substr(0, 10),
    dateFormatted: vm.formatDate(
      new Date(Date.now() - new Date().getTimezoneOffset() * 60000)
        .toISOString()
        .substr(0, 10)
    ),
    menu1: false,
    gaji: null,
    karyawan: [],
    showForm: false,
    modeForm: "Tambah",
    showTable: true,
    headers: [
      {
        text: "NIP",
        align: "start",
        sortable: false,
        value: "nip",
      },
      { text: "NIK", value: "nik" },
      { text: "Nama", value: "nama" },
      { text: "Alamat", value: "alamat" },
      { text: "Status Karyawan", value: "status_karyawan" },
      { text: "Actions", value: "actions", sortable: false },
    ],
  }),

  computed: {
    formTitle() {
      return this.editedIndex === -1 ? "New Item" : "Edit Item";
    },
    computedDateFormatted() {
      return this.formatDate(this.tanggal_masuk);
    },
  },

  watch: {
    tanggal_masuk(val) {
      this.dateFormatted = this.formatDate(this.tanggal_masuk);
    },
  },

  mounted() {
    this.getKaryawan();
  },

  methods: {
    formVisibility(valueModeForm = "Tambah", valueShowForm, valueShowTable) {
      this.modeForm = valueModeForm;
      this.showForm = valueShowForm;
      this.showTable = valueShowTable;
    },
    async getKaryawan() {
      const karyawan = await this.$axios("/karyawan");
      this.karyawan = karyawan.data;
      console.log(this.karyawan);
    },
    saveKaryawan() {
      if (this.modeForm == "Tambah") {
        const result = this.$axios.post("/karyawan", {
          nip: this.nip,
          nik: this.nik,
          nama: this.nama,
          alamat: this.alamat,
          jabatan: this.jabatan,
          status_karyawan: this.status_karyawan,
          status_pernikahan: this.status_pernikahan,
          tanggal_masuk: this.tanggal_masuk,
          gaji: this.gaji,
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
          .put("/karyawan", {
            nip: this.nip,
            nik: this.nik,
            nama: this.nama,
            alamat: this.alamat,
            jabatan: this.jabatan,
            status_karyawan: this.status_karyawan,
            status_pernikahan: this.status_pernikahan,
            tanggal_masuk: this.tanggal_masuk,
            gaji: this.gaji,
            id: this.id,
          })
          .then((result) => {
            this.clearAndRefreshForm(result);
          });
      }
    },
    editKaryawan(karyawan) {
      this.showTable = false;
      this.modeForm = "Ubah";
      this.showForm = true;
      this.nip = karyawan.nip;
      this.nik = karyawan.nik;
      this.nama = karyawan.nama;
      this.alamat = karyawan.alamat;
      this.jabatan = karyawan.jabatan;
      this.status_karyawan = karyawan.status_karyawan;
      this.status_pernikahan = karyawan.status_pernikahan;
      this.tanggal_masuk = karyawan.tanggal_masuk;
      this.gaji = karyawan.gaji;
      this.id = karyawan.id;
    },
    clearAndRefreshForm(result) {
      alert(result.data.message);
      this.formVisibility(false);
      this.nip = "";
      this.nik = "";
      this.nama = "";
      this.alamat = "";
      this.status_karyawan = "";
      this.status_pernikahan = "";
      this.tanggal_masuk = "";
      this.gaji = 0;
      this.showTable = true;
      this.getKaryawan();
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
