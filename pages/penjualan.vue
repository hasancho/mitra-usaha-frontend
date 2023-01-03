<template>
  <div>
    <v-card class="mt-10 pb-5">
      <v-divider></v-divider>
      <v-card class="ma-5" v-show="showForm">
        <v-card-title>{{ modeForm }} Data Penjualan</v-card-title>
        <div v-show="modeForm === 'Ubah' || modeForm === 'Tambah'">
          <v-form ref="form" class="pa-5" lazy-validation>
            <v-row>
              <v-col cols="4">
                <v-text-field
                  v-model="no_do"
                  label="NO. DO"
                  required
                ></v-text-field>
              </v-col>
              <!-- <v-col cols="4">
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
            </v-col> -->
              <v-col cols="12" sm="6" md="4">
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
            </v-row>
            <v-row>
              <v-col cols="4">
                <v-select
                  v-model="customer"
                  :items="listNamaCustomer"
                  label="CUSTOMER"
                  required
                ></v-select>
              </v-col>
              <v-col cols="4">
                <v-select
                  v-model="pengiriman"
                  :items="listIdPengiriman"
                  :rules="[(v) => !!v || 'Item is required']"
                  label="TUJUAN PENGIRIMAN"
                  :click="getTarif()"
                  required
                ></v-select>
              </v-col>
            </v-row>
            <v-row>
              <v-col cols="4">
                <v-select
                  v-model="kendaraan"
                  :items="listNoPolKendaraan"
                  label="KENDARAAN"
                  required
                ></v-select>
              </v-col>
            </v-row>
            <v-row>
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
                  label="QUANTITY"
                  required
                ></v-text-field> </v-col
              ><v-col cols="4"> </v-col>
            </v-row>
            <v-row>
              <v-col cols="4">
                <v-radio-group v-model="pembayaran" label="PEMBAYARAN">
                  <v-radio
                    v-for="(pilihan_pem, x) in status_pembayaran"
                    :key="x"
                    :label="pilihan_pem"
                    :value="pilihan_pem"
                    :pilihan="pilihan_pem"
                  ></v-radio>
                </v-radio-group>
              </v-col>
              <v-col cols="4">
                <v-radio-group v-model="status_pajak" label="STATUS PAJAK">
                  <v-radio
                    v-for="(pilihan_paj, i) in pilihan_status_pajak"
                    :key="i"
                    :label="pilihan_paj"
                    :value="pilihan_paj"
                    :pilihan="pilihan_paj"
                    :click="getPenjualanPPN()"
                  ></v-radio>
                </v-radio-group>
              </v-col>
            </v-row>
            <v-row>
              <v-col cols="4">
                <v-text-field
                  v-model="penjualan_sebelum_pajak"
                  label="PENJUALAN SEBELUM PAJAK"
                  prefix="Rp."
                  required
                  readonly
                ></v-text-field>
              </v-col>
              <v-col cols="4">
                <v-text-field
                  v-model="penjualan_sesudah_pajak"
                  label="PENJUALAN SESUDAH PAJAK"
                  prefix="Rp."
                  required
                  readonly
                ></v-text-field>
              </v-col>
            </v-row>
            <v-btn color="success" class="mr-4" v-on:click="savePenjualan">
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
              :filename="no_do"
              :pdf-quality="2"
              :manual-pagination="false"
              pdf-format="a4"
              pdf-orientation="landscape"
              pdf-content-width="800px"
              ref="html2Pdf"
            >
              <section slot="pdf-content">
                <div style="margin-top: 50px; margin-left: 300px">
                  <h1 style="color: brown; font-size: 30px">CV. MITRA USAHA</h1>
                  <p style="color: aqua">
                    GENERAL CONTRACTOR, SUPPLIER & TRANSPORTER
                  </p>
                  <hr />
                  <!-- <table>
                    <tr>
                      <td>No.DO</td>
                      <td>{{ no_do }}</td>
                    </tr>
                    <tr>
                      <td>Customer</td>
                      <td>{{ customer }}</td>
                    </tr>
                    <tr>
                      <td>Tujuan Pengiriman</td>
                      <td>{{ pengiriman }}</td>
                    </tr>
                  </table> -->
                  <p style="margin-bottom: 10px">No. Do: {{ no_do }}</p>
                  <p style="margin-bottom: 10px">Customer: {{ customer }}</p>
                  <p style="margin-bottom: 10px">
                    Tujuan Pengiriman: {{ pengiriman }}
                  </p>
                  <p style="margin-bottom: 10px">Kendaraan: {{ kendaraan }}</p>
                  <p style="margin-bottom: 10px">Quantity: {{ quantity }}</p>
                  <p style="margin-bottom: 10px">
                    Pembayaran: {{ pembayaran }}
                  </p>
                  <p style="margin-bottom: 10px">
                    Status Pajak: {{ status_pajak }}
                  </p>
                  <p style="margin-bottom: 10px">
                    Penjualan Sebelum Pajak: Rp. {{ penjualan_sebelum_pajak }}
                  </p>
                  <p style="margin-bottom: 10px">
                    Penjualan Sesudah Pajak: Rp. {{ penjualan_sesudah_pajak }}
                  </p>
                </div>
              </section>
            </vue-html2pdf>
          </client-only>
          <p>No. Do: {{ no_do }}</p>
          <p>Tanggal: {{ tanggal }}</p>
          <p>Customer: {{ customer }}</p>
          <p>Tujuan Pengiriman: {{ pengiriman }}</p>
          <p>Kendaraan: {{ kendaraan }}</p>
          <p>Quantity: {{ quantity }}</p>
          <p>Pembayaran: {{ pembayaran }}</p>
          <p>Status Pajak: {{ status_pajak }}</p>
          <p>Penjualan Sebelum Pajak: Rp. {{ penjualan_sebelum_pajak }}</p>
          <p>Penjualan Sesudah Pajak: Rp. {{ penjualan_sesudah_pajak }}</p>
          <v-btn color="primary" v-on:click="clearAndRefreshForm">
            <router-link
              to="/penjualan"
              style="color: white; text-decoration: none"
              >Back</router-link
            >
          </v-btn>
          <v-btn color="error" v-on:click="generateReport" class="ml-4"
            >Export to PDF
          </v-btn>
        </div>
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
          <div
            v-if="
              loggedInUser.data.role === 'admin' ||
              loggedInUser.data.role === 'superadmin'
            "
            style="float: left"
          >
            <v-icon small class="mr-2" @click="editPenjualan(item)">
              mdi-pencil
            </v-icon>
            <v-icon small class="mr-3" @click="deletePenjualan(item)">
              mdi-delete
            </v-icon>
          </div>
          <v-icon small class="mr-2" @click="viewPenjualan(item)">
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
    id: null,
    customer: "",
    listCustomer: [],
    listNamaCustomer: [],
    pengiriman: "",
    pengiriman_biaya_pokok: null,
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
    quantity: null,
    pembayaran: "",
    status_pajak: "",
    penjualan: [],
    tarif: 0,
    ppn: 0,
    showForm: false,
    showTable: true,
    modeForm: "Tambah",
    value: null,
    radioGroup: "",
    select: null,
    status_pembayaran: ["CASH", "KREDIT"],
    pilihan_status_pajak: ["PPN", "NON-PPN"],
    penjualan_sebelum_pajak: 0,
    penjualan_sesudah_pajak: 0,
    checkbox: false,
    // tanggal: new Date(Date.now() - new Date().getTimezoneOffset() * 60000)
    //   .toISOString()
    //   .substr(0, 10),
    // dateFormatted: vm.formatDate(
    //   new Date(Date.now() - new Date().getTimezoneOffset() * 60000)
    //     .toISOString()
    //     .substr(0, 10)
    // ),
    tanggal: new Date(Date.now() - new Date().getTimezoneOffset() * 60000)
      .toISOString()
      .substr(0, 10),
    menu: false,
    modal: false,
    showOption: true,
    username: "",
    headers: [
      {
        text: "No. Do",
        align: "start",
        sortable: false,
        value: "no_do",
      },
      { text: "Tanggal", value: "tanggal" },
      { text: "Customer", value: "nama" },
      { text: "Pengiriman", value: "tujuan" },
      // { text: "Kendaraan", value: "no_pol" },
      { text: "Total Penjualan", value: "penjualan_sesudah_pajak" },
      { text: "Actions", value: "actions", sortable: false },
    ],
  }),
  watch: {
    // tanggal(val) {
    //   this.dateFormatted = this.formatDate(this.tanggal);
    // },
    // total_biaya() {
    //   return this.pengiriman_biaya_pokok;
    // },
  },
  computed: {
    ...mapGetters(["isAuthenticated", "loggedInUser"]),
  },
  mounted() {
    this.getPenjualan();
    this.getCustomer();
    this.getPengiriman();
    this.getKendaraan();
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
        this.listNamaCustomer.push(
          this.listCustomer[i].id_customer + "-" + this.listCustomer[i].nama
        );
      }
    },
    async getPengiriman() {
      const getPengiriman = await this.$axios("/pengiriman");
      this.listPengiriman = getPengiriman.data;
      for (let i = 0; i < this.listPengiriman.length; i++) {
        this.listIdPengiriman.push(
          this.listPengiriman[i].id_pengiriman +
            "-" +
            this.listPengiriman[i].kode_tujuan +
            "-" +
            this.listPengiriman[i].tarif
        );
      }
    },
    async getKendaraan() {
      const getKendaraan = await this.$axios("/kendaraan");
      this.listKendaraan = getKendaraan.data;
      for (let i = 0; i < this.listKendaraan.length; i++) {
        this.listNoPolKendaraan.push(
          this.listKendaraan[i].id_kendaraan +
            "-" +
            this.listKendaraan[i].no_pol
        );
      }
    },
    getTarif() {
      const stringDelimiter = function (sampleInput, delimiter) {
        let stringArray = [""];
        let j = 0;

        for (let i = 0; i < sampleInput.length; i++) {
          if (sampleInput.charAt(i) == delimiter) {
            j++;
            stringArray.push("");
          } else {
            stringArray[j] += sampleInput.charAt(i);
          }
        }
        return stringArray;
      };
      this.tarif = stringDelimiter(this.pengiriman, "-")[3];
      // this.tarif = this.pengiriman.split("-")[3];
      this.penjualan_sebelum_pajak = Math.round(this.tarif * this.quantity);
      this.ppn = (this.penjualan_sebelum_pajak * 11) / 100;
    },
    getPenjualanPPN() {
      if (this.status_pajak == "PPN") {
        this.penjualan_sesudah_pajak = this.penjualan_sebelum_pajak + this.ppn;
        return this.penjualan_sesudah_pajak;
      }
      return (this.penjualan_sesudah_pajak = this.penjualan_sebelum_pajak);
    },
    // async test(id) {
    //   const getPengiriman = await this.$axios("/pengiriman");
    //   console.log(getPengiriman.data[id].biaya_pokok);
    // },
    // async getKaryawan() {
    //   const getKaryawan = await this.$axios("/karyawan");
    //   this.listKaryawan = getKaryawan.data;
    //   for (let i = 0; i < this.listKaryawan.length; i++) {
    //     this.listNamaKaryawan.push(this.listKaryawan[i].id);
    //   }
    // },
    // calculateTotalBiaya() {
    //   this.total_biaya = this.tarif * this.quantity;
    // },
    // async test() {
    //   const getPengiriman = await this.$axios("/pengiriman");
    //   this.listPengiriman = getPengiriman.data;
    //   for (let i = 0; i < this.listPengiriman.length; i++) {
    //     this.listIdPengiriman.push(
    //       this.listPengiriman[i].id + "-" + this.listPengiriman[i].kode_tujuan
    //     );
    //     console.log(this.listIdPengiriman);
    //   }
    //   console.log("Yoo");
    // },
    savePenjualan() {
      if (this.modeForm === "Tambah") {
        const result = this.$axios.post("/penjualan", {
          tanggal: this.tanggal,
          id_customer: parseInt(this.customer.split("-")),
          id_pengiriman: parseInt(this.pengiriman.split("-")),
          id_kendaraan: parseInt(this.kendaraan.split("-")),
          no_do: this.no_do,
          quantity: this.quantity,
          pembayaran: this.pembayaran,
          penjualan_sebelum_pajak: this.penjualan_sebelum_pajak,
          penjualan_sesudah_pajak: this.penjualan_sesudah_pajak,
          status_pajak: this.status_pajak,
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
            tanggal: this.tanggal,
            id_customer: parseInt(this.customer.split("-")[0]),
            id_pengiriman: parseInt(this.pengiriman.split("-")[0]),
            id_kendaraan: parseInt(this.kendaraan.split("-")[0]),
            no_do: this.no_do,
            quantity: this.quantity,
            pembayaran: this.pembayaran,
            status_pajak: this.status_pajak,
            penjualan_sebelum_pajak: this.penjualan_sebelum_pajak,
            penjualan_sesudah_pajak: this.penjualan_sesudah_pajak,
            id: this.id,
          })
          .then((result) => {
            this.clearAndRefreshForm(result);
          });
      } else if (this.modeForm === "Lihat") {
        this.$axios
          .put("/penjualan", {
            tanggal: this.tanggal,
            id_customer: parseInt(this.customer.split("-")[0]),
            id_pengiriman: parseInt(this.pengiriman.split("-")[0]),
            id_kendaraan: parseInt(this.kendaraan.split("-")[0]),
            no_do: this.no_do,
            quantity: this.quantity,
            pembayaran: this.pembayaran,
            status_pajak: this.status_pajak,
            penjualan_sebelum_pajak: this.penjualan_sebelum_pajak,
            penjualan_sesudah_pajak: this.penjualan_sesudah_pajak,
          })
          .then((result) => {
            alert(result.data.message);
            this.clearAndRefreshForm(result);
          });
      }
    },

    viewPenjualan(penjualan) {
      this.showTable = false;
      this.modeForm = "Lihat";
      this.showForm = true;
      this.tanggal = penjualan.tanggal;
      this.customer = penjualan.id_customer + "-" + penjualan.nama;
      this.pengiriman =
        penjualan.id_pengiriman +
        "-" +
        penjualan.kode_tujuan +
        "-" +
        penjualan.tarif;
      this.kendaraan = penjualan.id_kendaraan + "-" + penjualan.no_pol;
      this.no_do = penjualan.no_do;
      this.quantity = penjualan.quantity;
      this.pembayaran = penjualan.pembayaran;
      this.status_pajak = penjualan.status_pajak;
      this.penjualan_sebelum_pajak = penjualan.penjualan_sebelum_pajak;
      this.penjualan_sesudah_pajak = penjualan.penjualan_sesudah_pajak;
      this.id = penjualan.id;
    },
    editPenjualan(penjualan) {
      this.showTable = false;
      this.modeForm = "Ubah";
      this.showForm = true;
      this.tanggal = penjualan.tanggal;
      this.customer = penjualan.id_customer + "-" + penjualan.nama;
      this.pengiriman =
        penjualan.id_pengiriman +
        "-" +
        penjualan.kode_tujuan +
        "-" +
        penjualan.tarif;
      this.kendaraan = penjualan.id_kendaraan + "-" + penjualan.no_pol;
      this.no_do = penjualan.no_do;
      this.quantity = penjualan.quantity;
      this.pembayaran = penjualan.pembayaran;
      this.status_pajak = penjualan.status_pajak;
      this.penjualan_sebelum_pajak = penjualan.penjualan_sebelum_pajak;
      this.penjualan_sesudah_pajak = penjualan.penjualan_sesudah_pajak;
      this.id = penjualan.id;
    },
    deletePenjualan(penjualan) {
      this.$axios.delete("/penjualan/" + penjualan.id).then((response) => {
        alert(response.data.message);
        this.getPenjualan();
      });
    },
    clearAndRefreshForm(result) {
      this.formVisibilty(false);
      this.tanggal = null;
      this.customer = 0;
      this.pengiriman = 0;
      this.kendaraan = 0;
      this.no_do = "";
      this.quantity = 0;
      this.pembayaran = "";
      this.status_pajak = "";
      this.penjualan_sebelum_pajak = 0;
      this.penjualan_sesudah_pajak = 0;
      this.showTable = true;
      this.getPenjualan();
      // this.$router.push("/");
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
    // formatDate(date) {
    //   if (!date) return null;

    //   const [year, month, day] = date.split("-");
    //   return `${month}/${day}/${year}`;
    // },
    // parseDate(date) {
    //   if (!date) return null;

    //   const [month, day, year] = date.split("/");
    //   return `${year}-${month.padStart(2, "0")}-${day.padStart(2, "0")}`;
    // },
  },
};
</script>
