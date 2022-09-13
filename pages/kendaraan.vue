<template>
  <div>
    <v-card class="mt-10 pb-5">
      <v-card-title>Data Kendaraan</v-card-title>
      <v-divider></v-divider>
      <v-card class="ma-5">
        <v-form ref="form" class="pa-5" v-model="valid" lazy-validation>
          <v-row>
            <v-col cols="4">
              <v-text-field
                v-model="noPol"
                label="No. Pol"
                required
              ></v-text-field>
            </v-col>
            <v-col cols="4">
              <v-text-field
                v-model="tahun"
                label="Tahun"
                required
              ></v-text-field>
            </v-col>
          </v-row>
          <v-row>
            <v-col cols="4">
              <v-text-field
                v-model="jenis"
                label="Jenis"
                required
              ></v-text-field>
            </v-col>
            <v-col cols="4">
              <v-text-field v-model="tipe" label="Tipe" required></v-text-field>
            </v-col>
          </v-row>
          <v-row>
            <v-col cols="4">
              <v-text-field
                v-model="nomor_rangka"
                label="Nomer Rangka"
                required
              ></v-text-field>
            </v-col>
            <v-col cols="4">
              <v-text-field
                v-model="nomer_mesin"
                label="Nomer Mesin"
                required
              ></v-text-field>
            </v-col>
          </v-row>
          <v-row>
            <v-col cols="4">
              <v-text-field
                v-model="value"
                label="Masa Berlaku STNK"
                dense
                hide-details
                class="pa-5"
              >
                <template v-slot:append-outer>
                  <date-picker v-model="value" />
                </template>
              </v-text-field>
            </v-col>
            <v-col cols="4">
              <v-text-field
                v-model="value"
                label="Masa Berlaku Pajak"
                dense
                hide-details
                class="pa-5"
              >
                <template v-slot:append-outer>
                  <date-picker v-model="value" />
                </template>
              </v-text-field>
            </v-col>
          </v-row>
          <v-row>
            <v-col cols="4">
              <v-text-field
                v-model="value"
                label="Masa Berlaku Kir"
                dense
                hide-details
                class="pa-5"
              >
                <template v-slot:append-outer>
                  <date-picker v-model="value" />
                </template>
              </v-text-field>
            </v-col>
          </v-row>
          <v-btn color="success" class="mr-4"> Submit </v-btn>

          <v-btn color="error" class="mr-4" @click="reset"> Reset Form </v-btn>
        </v-form>
      </v-card>
    </v-card>
  </div>
</template>

<script>
import DatePickerVue from "~/components/DatePicker.vue";
export default {
  components: {
    DatePickerVue,
  },
  data: () => ({
    noPol: "",
    tahun: "",
    jenis: "",
    tipe: "",
    nomor_rangka: "",
    nomor_mesin: "",
    masa_berlaku_stnk: "",
    masa_berlaku_pajak: "",
    masa_berlaku_kir: "",
    valid: true,
    value: null,
  }),

  methods: {
    reset() {
      this.$refs.form.reset();
    },
  },
};
</script>
