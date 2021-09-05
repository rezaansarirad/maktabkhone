<template>
  <div class="d-flex flex-column">
    <mheader @search="search"></mheader>
    <v-container class="d-flex flex-column">
      <div class="d-flex justify-start">
        <v-btn color="#00bf6f" dark rounded @click="sortAsc()">
          بیشترین امتیاز
          <v-icon>arrow_upward</v-icon>
        </v-btn>
        <v-btn color="#ef394e" dark rounded @click="sortDesc()">
          کمترین امتیاز
          <v-icon>arrow_downward</v-icon>
        </v-btn>
      </div>
      <div class="d-flex justify-end">
        <v-btn
          color="#21254e"
          dark
          rounded
          @click="showAddDoctorDialog = !showAddDoctorDialog"
        >
          افزودن دکتر جدید
          <v-icon>add_circle</v-icon>
        </v-btn>
      </div>

      <add-doctor
        :visible="showAddDoctorDialog"
        @close="showAddDoctorDialog = false"
        @addDoctor="addDoctor"
      ></add-doctor>
      <div class="d-flex flex-row">
        <v-container fluid>
          <v-row>
            <v-col
              cols="12"
              lg="4"
              v-for="(item, i) in doctors"
              v-show="doctors"
              :key="i"
            >
              <doctor-item
                @liked="liked"
                :doctor="item"
                :index="i"
              ></doctor-item>
            </v-col>
          </v-row>
        </v-container>
      </div>
    </v-container>
  </div>
</template>



<script>
import doctorItem from "../components/doctorItem.vue";
export default {
  components: { doctorItem },
  data() {
    return {
      showAddDoctorDialog: false,
      doctors: [],
    };
  },
  methods: {
    addDoctor(item) {
      this.doctors.push(item);
      localStorage.setItem("session", JSON.stringify(this.doctors));
    },
    search(val) {
      if (this.doctors) {
        this.doctors = JSON.parse(localStorage.getItem("session"));
        this.doctors = this.doctors.filter((item) =>
          item.name.toLowerCase().includes(val.toLowerCase())
        );
      }
    },
    liked(val) {
      this.doctors[val.index].like += val.count;
      localStorage.setItem("session", JSON.stringify(this.doctors));
    },
    sortDesc() {
      if (this.doctors) {
        this.doctors = this.doctors.sort(function (a, b) {
          return a.like - b.like;
        });
      }
    },
    sortAsc() {
      if (this.doctors) {
        this.doctors = this.doctors
          .sort(function (a, b) {
            return a.like - b.like;
          })
          .reverse();
      }
    },
  },
  created() {
    if (process.browser) {
      this.doctors = JSON.parse(localStorage.getItem("session"))|| [];
    }
  },
};
</script>