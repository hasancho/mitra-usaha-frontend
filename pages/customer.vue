<template>
  <div>
    <v-spacer></v-spacer>
    <!-- <v-alert v-show="showAlert" dense text type="success">
      Berhasil tambah data
    </v-alert> -->
    <v-card class="mt-10 pb-5">
      <v-divider></v-divider>
      <v-card class="ma-5" v-show="showForm">
        <v-card-title>{{ modeForm }} Data Customer</v-card-title>
        <div v-show="modeForm === 'Ubah' || modeForm === 'Tambah'">
          <v-form ref="form" class="pa-5" lazy-validation>
            <v-row>
              <v-col cols="4">
                <v-text-field
                  v-model="npwp"
                  label="NPWP"
                  required
                ></v-text-field>
              </v-col>
              <v-col cols="4">
                <v-text-field
                  v-model="nama"
                  label="Nama"
                  required
                ></v-text-field>
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
            <v-btn color="error" class="mr-4" @click="reset">
              Reset Form
            </v-btn>
          </v-form>
        </div>
        <div class="pa-5" v-if="modeForm === 'Lihat'">
          <client-only>
            <vue-html2pdf
              :show-layout="false"
              :float-layout="true"
              :enable-download="true"
              :preview-modal="false"
              :paginate-elements-by-height="1400"
              :filename="npwp"
              :pdf-quality="2"
              :manual-pagination="false"
              pdf-format="a4"
              pdf-orientation="landscape"
              pdf-content-width="800px"
              ref="html2Pdf"
            >
              <section slot="pdf-content">
                <div style="margin: 200px">
                  <p>NPWP: {{ npwp }}</p>
                  <p>Nama: {{ nama }}</p>
                  <p>Alamat: {{ alamat }}</p>
                  <p>No Telepon: {{ noTelepon }}</p>
                </div>
              </section>
            </vue-html2pdf>
          </client-only>
          <p>NPWP: {{ npwp }}</p>
          <p>Nama: {{ nama }}</p>
          <p>Alamat: {{ alamat }}</p>
          <p>No Telepon: {{ noTelepon }}</p>
          <v-btn color="primary" v-on:click="clearAndRefreshForm">
            <router-link
              to="/customer"
              style="color: white; text-decoration: none"
              >Back</router-link
            >
          </v-btn>
          <!-- <v-btn color="error" v-on:click="generateReport" class="ml-4"
            >Export to PDF
          </v-btn> -->
        </div>
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
          <div
            v-if="loggedInUser.data.role === 'superadmin'"
            style="float: left"
          >
            <v-icon small class="mr-2" @click="editCustomer(item)">
              mdi-pencil
            </v-icon>
            <v-icon small class="mr-3" @click="deleteCustomer(item)">
              mdi-delete
            </v-icon>
          </div>
          <v-icon small class="mr-2" @click="viewCustomer(item)">
            mdi-eye
          </v-icon>
        </template>
      </v-data-table>
    </v-card>
  </div>
</template>

<script>
import { mapGetters } from "vuex";
import VueHtml2pdf from "vue-html2pdf";
export default {
  components: {
    VueHtml2pdf,
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
  computed: {
    ...mapGetters(["isAuthenticated", "loggedInUser"]),
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
            alert(result.data.message);
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
            alert(result.data.message);
            this.clearAndRefreshForm(result);
          });
      } else if (this.modeForm === "Lihat") {
        this.$axios
          .put("/customer", {
            npwp: this.npwp,
            nama: this.nama,
            alamat: this.alamat,
            no_telepon: this.noTelepon,
            id_customer: this.id_customer,
          })
          .then((result) => {
            alert(result.data.message);
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

    clearAndRefreshForm() {
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

    generateReport() {
      this.$refs.html2Pdf.generatePdf();
    },

    async beforeDownload({ html2pdf, options, pdfContent }) {
      await html2pdf()
        .set(options)
        .from(pdfContent)
        .toPdf()
        .get("pdf")
        .then((pdf) => {
          const totalPages = pdf.internal.getNumberOfPages();
          for (let i = 1; i <= totalPages; i++) {
            pdf.setPage(i);
            pdf.setFontSize(10);
            pdf.setTextColor(150);
            pdf.text(
              "Page " + i + " of " + totalPages,
              pdf.internal.pageSize.getWidth() * 0.88,
              pdf.internal.pageSize.getHeight() - 0.3
            );
          }
        })
        .save();
    },
  },
};
</script>
