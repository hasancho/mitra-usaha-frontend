<template>
  <div>
    <v-card class="mt-10 pb-5">
      <v-card-title>Data Karyawan</v-card-title>
      <v-divider></v-divider>
      <v-card class="ma-5">
        <v-form ref="form" class="pa-5" v-model="valid" lazy-validation>
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
              <v-text-field
                v-model="tanggal_masuk"
                label="Tanggal Masuk"
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
            <v-col cols="3">
              <v-radio-group v-model="radioGroup" label="Status Karyawan">
                <v-radio
                  v-for="s in status_karyawan"
                  :key="s"
                  :label="s.value"
                  :value="s.value"
                ></v-radio>
              </v-radio-group>
            </v-col>
            <v-col cols="3">
              <v-radio-group
                v-model="status_pernikahan"
                label="Status Pernikahan"
              >
                <v-radio
                  v-for="s in status_pernikahan"
                  :key="s"
                  :label="s.value"
                  :value="s.value"
                ></v-radio>
              </v-radio-group>
            </v-col>
            <v-col cols="4">
              <v-text-field v-model="gaji" label="Gaji" required></v-text-field>
            </v-col>
          </v-row>

          <v-select
            v-model="select"
            :items="items"
            label="Item"
            required
          ></v-select>

          <v-btn
            :disabled="!valid"
            color="success"
            class="mr-4"
            @click="validate"
          >
            Submit
          </v-btn>

          <v-btn color="error" class="mr-4" @click="reset"> Reset Form </v-btn>
        </v-form>
      </v-card>
    </v-card>
  </div>
</template>

<script>
import DatePicker from "../components/DatePicker.vue";
export default {
  components: {
    DatePicker,
  },
  data: () => ({
    nip: "",
    nik: "",
    nama: "",
    alamat: "",
    status_karyawan: [{ value: "tetap" }, { value: "tidak tetap" }],
    status_pernikahan: [{ value: "sudah" }, { value: "belum" }],
    tanggal_masuk: "",
    gaji: null,
    radioGroup: "",
    select: null,
    items: ["Item 1", "Item 2", "Item 3", "Item 4"],
  }),

  methods: {
    reset() {
      this.$refs.form.reset();
    },
  },
};
</script>
