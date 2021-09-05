<template>
  <div class="text-center">
    <v-dialog v-model="show" max-width="400" transition="dialog-top-transition">
      <v-card elevation="0" style="padding: 10px">
        <div class="d-flex flex-column">
          <div class="d-flex flex-row justify-space-between align-center">
            <div class="d-flex flex-row justify-center align-center">
              <v-icon color="#00b894">add</v-icon>
              <span class="info-new-span">اطلاعات دکتر جدید </span>
            </div>
            <div>
              <v-icon color="#757575" @click="show = false">close</v-icon>
            </div>
          </div>
        </div>
        <v-form
          ref="doctorForm"
          v-model="valid"
          style="padding: 0px"
          lazy-validation
        >
          <v-col cols="12" class="d-flex flex-wrap">
            <v-col cols="12" md="12" lg="12" class="padding-4">
              <v-text-field
                v-model="doctor.name"
                outlined
                shaped
                label="نام شما"
                type="text"
                hint="لطفا نام را بصورت انگلیسی وارد کنید"
                onkeypress="return (event.charCode >= 65 && event.charCode <= 90) || (event.charCode >= 97 && event.charCode <= 122) || (event.charCode >= 48 && event.charCode <= 57)"
                :rules="nameRule"
              ></v-text-field>
            </v-col>

            <v-col cols="12" md="12" lg="12" class="padding-4">
              <v-text-field
                v-model="doctor.email"
                label="ایمیل شما"
                outlined
                :rules="[rules.required, rules.email]"
                color="#000"
                hint="info@gmail.com"
                shaped
              ></v-text-field>
            </v-col>
            <v-col cols="12" md="12" lg="12" class="padding-4">
              <v-textarea
                v-model="doctor.description"
                outlined
                shaped
                label="توضیحات مختصری از شما"
                :rules="desRule"
              ></v-textarea>
            </v-col>
            <v-col cols="12" md="12" lg="12" class="padding-4">
              <span class="info-new-span"> انتخاب تخصص </span>

              <div>
                <v-chip-group
                  v-model="doctor.specialisations"
                  :rules="specRule"
                  column
                >
                  <v-chip filter-icon="done" filter value="Surgeon">
                    جراح
                  </v-chip>
                  <v-chip filter-icon="done" filter value="Radiologist">
                    متخصص رادیولوژی
                  </v-chip>
                  <v-chip filter-icon="done" filter value="Cardiologist">
                    متخصص قلب و عروق
                  </v-chip>
                  <v-chip filter-icon="done" filter value="Psychiatrist">
                    روانپزشک
                  </v-chip>
                  <v-chip filter-icon="done" filter value="Dermatologist">
                    متخصص پوست
                  </v-chip>
                </v-chip-group>
              </div>
            </v-col>
          </v-col>
        </v-form>
        <v-divider></v-divider>
        <div class="d-flex align-center">
          <v-btn
            color="#2b4a83"
            v-if="!isloading"
            rounded
            dark
            class="continue-btn"
            @click="addNewdoctor()"
          >
            افزودن دکتر جدید
            <v-icon>add_circle</v-icon>
          </v-btn>
          <v-progress-circular
            v-if="isloading"
            indeterminate
            color="purple"
          ></v-progress-circular>
        </div>
      </v-card>
    </v-dialog>
  </div>
</template>
<script>
export default {
  props: ["visible"],

  data() {
    return {
      doctor: {
        name: "",
        email: "",
        description: "",
        specialisations: "",
        like: 0,
      },
      valid: false,
      isloading: false,

      rules: {
        required: (value) => !!value || "وارد کردن این مقدار الزامی است!",
        email: (value) => {
          const pattern =
            /^(([^<>()[\]\\.,;:\s@"]+(\.[^<>()[\]\\.,;:\s@"]+)*)|(".+"))@((\[[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\])|(([a-zA-Z\-0-9]+\.)+[a-zA-Z]{2,}))$/;
          return pattern.test(value) || "لطفا ایمیل درست را وارد کنید";
        },
      },
      nameRule: [(v) => !!v || "نام فیلدی اجباری است"],
      desRule: [(v) => !!v || "توضیحات فیلدی اجباری است"],
      specRule: [(v) => !!v || "تخصص فیلدی اجباری است"],
    };
  },
  computed: {
    show: {
      get() {
        if (this.visible) {
        }
        return this.visible;
      },
      set(value) {
        if (!value) {
          this.params = "";
          this.$emit("close");
        }
      },
    },
  },
  methods: {
    clearlist() {
      this.doctor = {
        name: "",
        email: "",
        description: "",
        specialisations: "",
        like: 0,
      };
    },
    addNewdoctor() {
      if (this.$refs.doctorForm.validate()) {
        this.isloading = true;
        try {
        
          this.$emit("addDoctor", this.doctor);
          this.isloading = false;
          this.$emit("close");
          this.clearlist();
          this.$refs.doctorForm.resetValidation();
          this.$notify.success({
            title: "تایید",
            message: "دکترجدید با موفقیت اضافه شد!",
          });
        } catch (error) {
          console.log(error);
          this.$notify.error({
            title: "خطا",
            message: "خطایی رخ داده است لطفا مجددا تلاش کنید!",
          });
        }
      } else {
        this.$notify.error({
          title: "خطا",
          message: "لطفا تمام فیلد های اجباری را وارد کنید",
        });
        this.isloading = false;
      }
    },
  },

  created() {},
};
</script>

<style scoped>
.padding-4 {
  padding: 4px;
}
.info-new-span {
  font-family: vazir-bold;
  font-size: 18px;
  color: #2d3436;
  margin-right: 10px;
}
.continue-btn {
  margin-top: 20px;
}
</style>