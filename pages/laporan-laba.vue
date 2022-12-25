<template>
  <div>
    <v-card class="mt-10 pb-5">
      <v-card-title>Laporan Laba</v-card-title>
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
        </v-row>
        <v-row v-show="showTable">
          <v-col cols="12">
            <template>
              <v-simple-table>
                <template v-slot:default>
                  <thead id="tableData">
                    <tr>
                      <th>Tanggal</th>
                      <th class="text-left">PENJUALAN SEBELUM PAJAK</th>
                      <th></th>
                      <th class="text-left">BIAYA POKOK</th>
                    </tr>
                  </thead>
                  <tbody>
                    <tr v-for="item in penjualan" :key="item.id">
                      <td>{{ item.tanggal }}</td>
                      <td>Rp.{{ item.penjualan_sebelum_pajak }}</td>
                      <td></td>
                      <td>Rp.{{ item.biaya_pokok }}</td>
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
                      <td></td>
                      <td>Rp.{{ total_penjualan_sebelum_pajak }}</td>
                      <td style="font-weight: bold">-</td>
                      <td>Rp.{{ total_biaya_pokok }}</td>
                    </tr>
                  </tbody>
                  <tbody>
                    <tr>
                      <td>Total Laba:</td>
                      <td></td>
                      <td>Rp.{{ total_laba }}</td>
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
export default {
  data: () => ({
    dates: ["2022-01-01", "2022-01-02"],
    showTable: false,
    penjualan: [],
    total_penjualan_sebelum_pajak: 0,
    total_biaya_pokok: 0,
    total_laba: 0,
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
      const resultTotalLaba = await this.$axios.post(
        "/laporan-laba/total-laba",
        {
          from_tanggal: this.dates[0] + "T13:00:00",
          to_tanggal: this.dates[1] + "T13:00:00",
        }
      );
      this.penjualan = resultPenjualan.data;
      this.total_penjualan_sebelum_pajak =
        resultTotalLaba.data[0].total_penjualan_sebelum_pajak;
      this.total_biaya_pokok = resultTotalLaba.data[0].total_biaya_pokok;
      this.total_laba = resultTotalLaba.data[0].total_laba;
      // const resultTotalPenjualan = await this.$axios.get("/laporan-penjualan");
      // this.total_penjualan = resultTotalPenjualan.data[0].sum;
      // console.log(this.total_penjualan);
    },
  },
};
</script>
<style lang=""></style>
