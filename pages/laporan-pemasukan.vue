<template>
  <div>
    <v-card class="mt-10 pb-5">
      <v-card-title>Laporan Pemasukan Kas</v-card-title>
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
              </v-col>
              <v-btn color="success" class="mb-5 ml-7" v-on:click="getPemasukan"
                >Tampilkan</v-btn
              >
            </v-row>
          </template>
        </v-row>
        <v-row v-show="showTable">
          <v-col cols="12">
            <template>
              <v-simple-table>
                <template v-slot:default>
                  <thead>
                    <tr>
                      <th class="text-left">Tanggal</th>
                      <th class="text-left">Katerangan</th>
                      <th class="text-left">Total</th>
                    </tr>
                  </thead>
                  <tbody>
                    <tr v-for="item in pemasukan" :key="item.id">
                      <td>{{ item.tanggal }}</td>
                      <td>{{ item.keterangan }}</td>
                      <td>{{ item.total_pemasukan_kas }}</td>
                    </tr>
                  </tbody>
                  <br />
                  <thead>
                    <tr></tr>
                  </thead>
                  <tbody>
                    <tr>
                      <td>Total Pemasukan:</td>
                      <td></td>
                      <td>Rp.{{ total_pemasukan }}</td>
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
    pemasukan: [],
    showTable: false,
    total_pemasukan: 0,
  }),
  computed: {
    dateRangeText() {
      return this.dates;
    },
  },
  methods: {
    async getPemasukan() {
      this.showTable = true;
      const resultPemasukan = await this.$axios.post("/laporan-pemasukan", {
        from_tanggal: this.dates[0] + "T13:00:00",
        to_tanggal: this.dates[1] + "T13:00:00",
      });
      const resultTotalPemasukan = await this.$axios.post(
        "/laporan-pemasukan/total-pemasukan",
        {
          from_tanggal: this.dates[0] + "T13:00:00",
          to_tanggal: this.dates[1] + "T13:00:00",
        }
      );
      this.pemasukan = resultPemasukan.data;
      this.total_pemasukan = resultTotalPemasukan.data[0].sum;
    },
  },
};
</script>
<style lang=""></style>
