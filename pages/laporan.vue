<template>
  <div>
    <v-card class="mt-10 pb-5">
      <v-card-title>Laporan</v-card-title>
      <v-divider></v-divider>
      <!-- <v-card class="ma-5"> -->
      <v-row>
        <v-col cols="4" class="d-flex justify-space-between">
          <v-card class="ma-10" max-width="344">
            <div class="card" style="width: 200px">
              <v-card-text>
                <v-icon large color="green darken-2">
                  mdi-chart-box-outline
                </v-icon>
                <p class="text-h6 text--primary">Pemasukan</p>
              </v-card-text>
              <v-card-actions>
                <v-btn text color="teal accent-4" @click="reveal = true">
                  Lihat
                </v-btn>
              </v-card-actions>
            </div>
          </v-card>
          <v-card class="ma-10" width="344">
            <div class="card" style="width: 200px">
              <v-card-text>
                <v-icon large color="green darken-2">
                  mdi-chart-box-outline
                </v-icon>
                <p class="text-h6 text--primary">Pengeluaran</p>
              </v-card-text>
              <v-card-actions>
                <v-btn text color="teal accent-4" @click="reveal = true">
                  Lihat
                </v-btn>
              </v-card-actions>
            </div>
          </v-card>
          <v-card class="ma-10" max-width="344">
            <div class="card" style="width: 200px">
              <v-card-text>
                <v-icon large color="green darken-2">
                  mdi-chart-box-outline
                </v-icon>
                <p class="text-h6 text--primary">Laba</p>
              </v-card-text>
              <v-card-actions>
                <v-btn text color="teal accent-4" @click="reveal = true">
                  Lihat
                </v-btn>
              </v-card-actions>
            </div>
          </v-card>
        </v-col>
      </v-row>
      <v-row>
        <v-col cols="6" class="d-flex justify-space-between">
          <v-card class="ma-10">
            <div style="width: 200px">
              <v-card-text>
                <v-icon large color="green darken-2">
                  mdi-chart-box-outline
                </v-icon>
                <p class="text-h6 text--primary">Neraca</p>
              </v-card-text>
              <v-card-actions>
                <v-btn text color="teal accent-4" @click="reveal = true">
                  Lihat
                </v-btn>
              </v-card-actions>
            </div>
          </v-card>
          <v-card class="ma-10">
            <div style="width: 200px">
              <v-card-text>
                <v-icon large color="green darken-2">
                  mdi-chart-box-outline
                </v-icon>
                <p class="text-h6 text--primary">Arus Kas</p>
              </v-card-text>
              <v-card-actions>
                <v-btn text color="teal accent-4" @click="reveal = true">
                  Lihat
                </v-btn>
              </v-card-actions>
            </div>
          </v-card>
          <v-card class="ma-10">
            <div style="width: 200px">
              <v-card-text>
                <v-icon large color="green darken-2">
                  mdi-chart-box-outline
                </v-icon>
                <p class="text-h6 text--primary">Penjualan</p>
              </v-card-text>
              <v-card-actions>
                <v-btn text color="teal accent-4" @click="reveal = true">
                  Lihat
                </v-btn>
              </v-card-actions>
            </div>
          </v-card>
        </v-col>
      </v-row>
      <!-- </v-card> -->
    </v-card>
  </div>
</template>
