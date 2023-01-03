<template>
  <div>
    <v-card class="mt-10 pb-5">
      <v-divider></v-divider>
      <v-card class="ma-5" v-show="showForm">
        <v-card-title>{{ modeForm }} Data Customer</v-card-title>
        <div v-show="modeForm === 'Ubah' || modeForm === 'Tambah'">
          <v-form ref="form" class="pa-5" lazy-validation>
            <v-row>
              <v-col cols="4">
                <v-menu
                  ref="menu"
                  v-model="menu"
                  :close-on-content-click="false"
                  :return-value.sync="tanggal"
                  transition="scale-transition"
                  offset-y
                  min-width="auto"
                >
                  <template v-slot:activator="{ on, attrs }">
                    <v-text-field
                      v-model="tanggal"
                      label="Tanggal"
                      prepend-icon="mdi-calendar"
                      readonly
                      v-bind="attrs"
                      v-on="on"
                    ></v-text-field>
                  </template>
                  <v-date-picker v-model="tanggal" no-title scrollable>
                    <v-spacer></v-spacer>
                    <v-btn text color="primary" @click="menu = false">
                      Cancel
                    </v-btn>
                    <v-btn
                      text
                      color="primary"
                      @click="$refs.menu.save(tanggal)"
                    >
                      OK
                    </v-btn>
                  </v-date-picker>
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
                  v-model="total_pemasukan"
                  label="Total"
                  prefix="Rp."
                  required
                ></v-text-field>
              </v-col>
              <!-- <v-col cols="4">
              <v-text-field v-model="akun" label="Akun" required></v-text-field>
            </v-col> -->
            </v-row>
            <v-btn color="success" class="mr-4" v-on:click="savePemasukanKas">
              Submit
            </v-btn>
            <v-btn color="error" class="mr-4" @click="reset">
              Reset Form
            </v-btn>
          </v-form>
        </div>
        <div class="pa-5" v-if="modeForm === 'Lihat'">
          <p>Tanggal: {{ tanggal }}</p>
          <p>Keterangan: {{ keterangan }}</p>
          <p>Total Pemasukan: {{ total_pemasukan }}</p>
          <v-btn color="primary" v-on:click="clearAndRefreshForm">
            <router-link
              to="/pemasukan_kas"
              style="color: white; text-decoration: none"
              >Back</router-link
            >
          </v-btn>
        </div>
      </v-card>
      <v-data-table
        v-show="showTable"
        :headers="headers"
        :items="pemasukan_kas"
        sort-by="calories"
        class="elevation-1"
      >
        <template v-slot:top>
          <v-toolbar flat>
            <v-toolbar-title>Data Pemasukan Kas</v-toolbar-title>
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
          <div
            v-if="
              loggedInUser.data.role === 'admin' ||
              loggedInUser.data.role === 'superadmin'
            "
            style="float: left"
          >
            <v-icon small class="mr-2" @click="editPemasukanKas(item)">
              mdi-pencil
            </v-icon>
            <v-icon small class="mr-3" @click="deletePemasukanKas(item)">
              mdi-delete
            </v-icon>
          </div>
          <v-icon small class="mr-2" @click="viewPemasukanKas(item)">
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
    id: null,
    tanggal: "",
    keterangan: "",
    total_pemasukan: null,
    pemasukan_kas: [],
    akun: "",
    showForm: false,
    showTable: true,
    modeForm: "Tambah",
    // tanggal: new Date(Date.now() - new Date().getTimezoneOffset() * 60000)
    //   .toISOString()
    //   .substr(0, 10),
    // dateFormatted: value.formatDate(
    //   new Date(Date.now() - new Date().getTimezoneOffset() * 60000)
    //     .toISOString()
    //     .substr(0, 10)
    // ),
    tanggal: new Date(Date.now() - new Date().getTimezoneOffset() * 60000)
      .toISOString()
      .substr(0, 10),
    menu: false,
    modal: false,
    headers: [
      {
        text: "Tanggal",
        align: "start",
        sortable: false,
        value: "tanggal",
      },
      { text: "Keterangan", value: "keterangan" },
      { text: "Total Pemasukan", value: "total_pemasukan_kas" },
      { text: "Actions", value: "actions", sortable: false },
    ],
  }),
  watch: {
    // tanggal(val) {
    //   this.dateFormatted = this.formatDate(this.tanggal);
    // },
  },
  computed: {
    ...mapGetters(["isAuthenticated", "loggedInUser"]),
  },
  mounted() {
    this.getPemasukanKas();
  },
  methods: {
    formVisibilty(valueForm = "Tambah", valueShowForm, valueShowTable) {
      this.modeForm = valueForm;
      this.showForm = valueShowForm;
      this.showTable = valueShowTable;
    },
    async getPemasukanKas() {
      const getPemasukanKas = await this.$axios("/pemasukan_kas");
      this.pemasukan_kas = getPemasukanKas.data;
    },
    savePemasukanKas() {
      if (this.modeForm == "Tambah") {
        const result = this.$axios.post("/pemasukan_kas", {
          tanggal: this.tanggal,
          keterangan: this.keterangan,
          total_pemasukan_kas: this.total_pemasukan,
        });
        return result
          .then((result) => {
            alert(result.data.message);
            this.clearAndRefreshForm(result);
          })
          .catch((error) => {
            console.log(error);
          });
      } else if (this.modeForm === "Ubah") {
        this.$axios
          .put("/pemasukan_kas", {
            tanggal: this.tanggal,
            keterangan: this.keterangan,
            total_pemasukan_kas: this.total_pemasukan,
            id: this.id,
          })
          .then((result) => {
            alert(result.data.message);
            this.clearAndRefreshForm(result);
          });
      }
    },
    viewPemasukanKas(pemasukanKas) {
      this.showTable = false;
      this.modeForm = "Lihat";
      this.showForm = true;
      this.tanggal = pemasukanKas.tanggal;
      this.keterangan = pemasukanKas.keterangan;
      this.total_pemasukan = pemasukanKas.total_pemasukan_kas;
      this.id = pemasukanKas.id;
    },
    editPemasukanKas(pemasukanKas) {
      this.showTable = false;
      this.modeForm = "Ubah";
      this.showForm = true;
      this.tanggal = pemasukanKas.tanggal;
      this.keterangan = pemasukanKas.keterangan;
      this.total_pemasukan = pemasukanKas.total_pemasukan_kas;
      this.id = pemasukanKas.id;
    },
    clearAndRefreshForm(result) {
      this.formVisibilty(false);
      this.tanggal = "";
      this.keterangan = "";
      this.total_pemasukan = 0;
      this.showTable = true;
      this.getPemasukanKas();
    },
    deletePemasukanKas(pemasukanKas) {
      this.$axios
        .delete("/pemasukan_kas/" + pemasukanKas.id)
        .then((response) => {
          alert(response.data.message);
          this.getPemasukanKas();
        });
    },
    reset() {
      this.$refs.form.reset();
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
  },
};
</script>
