<template>
  <div>
    <v-card class="mt-10 pb-5">
      <v-card-title>Laporan Penjualan</v-card-title>
      <v-divider></v-divider>
      <v-card class="ma-5">
        <v-row>
          <template>
            <v-row>
              <v-col cols="12" sm="6">
                <v-date-picker v-model="dates" range></v-date-picker>
              </v-col>
              <v-col cols="12" sm="6">
                <v-text-field
                  v-model="dateRangeText"
                  label="Date range"
                  prepend-icon="mdi-calendar"
                  readonly
                ></v-text-field>
                <!-- model: {{ dates }} -->
              </v-col>
              <v-btn color="success" class="mb-5 ml-7" v-on:click="getPenjualan"
                >Tampilkan</v-btn
              >
              <!-- <v-btn
                color="error"
                class="mb-5 ml-7"
                v-on:click="exportToExcel('tableData', 'members-data')"
                >Export to Excel</v-btn
              > -->
            </v-row>
          </template>
          <!-- <template>
            <div>
              <vue-html2pdf
                :show-layout="false"
                :float-layout="true"
                :enable-download="true"
                :preview-modal="true"
                :paginate-elements-by-height="1400"
                filename="hee hee"
                :pdf-quality="2"
                :manual-pagination="false"
                pdf-format="a4"
                pdf-orientation="landscape"
                pdf-content-width="800px"
                @progress="onProgress($event)"
                @hasStartedGeneration="hasStartedGeneration()"
                @hasGenerated="hasGenerated($event)"
                ref="html2Pdf"
              >
                <section slot="pdf-content">
                </section>
              </vue-html2pdf>
            </div>
          </template> -->
        </v-row>
        <v-row v-show="showTable">
          <v-col cols="12">
            <template>
              <v-simple-table>
                <template v-slot:default>
                  <thead id="tableData">
                    <tr>
                      <th class="text-left">KODE JO</th>
                      <th class="text-left">NO DO</th>
                      <th class="text-left">QUANTITY</th>
                      <th class="text-left">PENJUALAN SEBELUM PAJAK</th>
                      <th class="text-left">PENJUALAN SESUDAH PAJAK</th>
                    </tr>
                  </thead>
                  <tbody>
                    <tr v-for="item in penjualan" :key="item.id">
                      <td>{{ item.kode_jo }}</td>
                      <td>{{ item.no_do }}</td>
                      <td>{{ item.quantity }}</td>
                      <td>Rp.{{ item.penjualan_sebelum_pajak }}</td>
                      <td>Rp.{{ item.penjualan_sesudah_pajak }}</td>
                    </tr>
                  </tbody>
                  <br />
                  <thead>
                    <tr>
                      <!-- <th class="text-left">Total Penjualan</th> -->
                    </tr>
                  </thead>
                  <tbody>
                    <tr>
                      <td>Total Penjualan:</td>
                      <td></td>
                      <td></td>
                      <td>Rp.{{ total_penjualan_sebelum_pajak }}</td>
                      <td>Rp.{{ total_penjualan_sesudah_pajak }}</td>
                    </tr>
                  </tbody>
                </template>
              </v-simple-table>
            </template>
          </v-col>
        </v-row>
      </v-card>
    </v-card>
  </div>
</template>
<script>
import VueHtml2pdf from "vue-html2pdf";
export default {
  components: {
    VueHtml2pdf,
  },
  data: () => ({
    dates: ["2022-01-01", "2022-01-02"],
    showTable: false,
    penjualan: [],
    total_penjualan_sebelum_pajak: 0,
    total_penjualan_sesudah_pajak: 0,
  }),
  computed: {
    dateRangeText() {
      return this.dates;
    },
  },
  methods: {
    async getPenjualan() {
      this.showTable = true;
      const resultPenjualan = await this.$axios.post("/laporan-penjualan", {
        from_tanggal: this.dates[0] + "T13:00:00",
        to_tanggal: this.dates[1] + "T13:00:00",
      });
      const resultTotalPenjualan = await this.$axios.post(
        "/laporan-penjualan/total-penjualan",
        {
          from_tanggal: this.dates[0] + "T13:00:00",
          to_tanggal: this.dates[1] + "T13:00:00",
        }
      );
      this.penjualan = resultPenjualan.data;
      this.total_penjualan_sebelum_pajak =
        resultTotalPenjualan.data[0].penjualan_sebelum_pajak;
      this.total_penjualan_sesudah_pajak =
        resultTotalPenjualan.data[0].penjualan_sesudah_pajak;
      // const resultTotalPenjualan = await this.$axios.get("/laporan-penjualan");
      // this.total_penjualan = resultTotalPenjualan.data[0].sum;
      // console.log(this.total_penjualan);
    },
    // async getTotalPenjualan() {
    //   const result = await this.$axios.get("/laporan-penjualan");
    //   this.total_penjualan = result.data;
    // },
    // exportToExcel(tableID, filename = "") {
    //   var downloadLink;
    //   var dataType = "application/vnd.ms-excel";
    //   var tableSelect = document.getElementById(tableID);
    //   var tableHTML = tableSelect.outerHTML.replace(/ /g, "%20");

    //   // Specify file name
    //   filename = filename ? filename + ".xls" : "excel_data.xls";

    //   // Create download link element
    //   downloadLink = document.createElement("a");

    //   document.body.appendChild(downloadLink);

    //   if (navigator.msSaveOrOpenBlob) {
    //     var blob = new Blob(["\ufeff", tableHTML], {
    //       type: dataType,
    //     });
    //     navigator.msSaveOrOpenBlob(blob, filename);
    //   } else {
    //     // Create a link to the file
    //     downloadLink.href = "data:" + dataType + ", " + tableHTML;

    //     // Setting the file name
    //     downloadLink.download = filename;

    //     //triggering the function
    //     downloadLink.click();
    //   }
    // },
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
<style lang=""></style>
