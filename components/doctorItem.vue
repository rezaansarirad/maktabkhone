<template>
 

  <v-card outlined>
     <v-col
        cols="12"
        lg="4"
        v-for="n in 6"
        :key="n"
        v-show="doctor && doctor.length <= 0"
      >
        <v-skeleton-loader
          class="mx-auto"
          max-width="300"
          type="card"
        ></v-skeleton-loader>
      </v-col>
    <v-list-item three-line>
      <v-list-item-content>
        <div v-show="doctor.specialisations === 'Surgeon'">جراح</div>
        <div v-show="doctor.specialisations === 'Radiologist'">
          متخصص رادیولوژی
        </div>
        <div v-show="doctor.specialisations === 'Cardiologist'">
          متخصص قلب و عروق
        </div>
        <div v-show="doctor.specialisations === 'Psychiatrist'">روانپزشک</div>
        <div v-show="doctor.specialisations === 'Dermatologist'">
          متخصص پوست
        </div>

        <v-list-item-title class="text-h5 mb-1">
          {{ doctor.name }}
        </v-list-item-title>
        <v-list-item-subtitle>
          {{ doctor.description }}
        </v-list-item-subtitle>
      </v-list-item-content>

      <v-list-item-avatar tile size="80" color="grey"></v-list-item-avatar>
    </v-list-item>
  <v-divider></v-divider>
    <v-card-actions class="d-flex justify-space-between align-items-center">
    
      <div >
        <span class="font-bold">{{ doctor.like }}</span>
      </div>

      <div class="margin" >
        <v-radio-group hide-details row>
          <v-radio
            on-icon="thumb_up"
            off-icon="thumb_up"
            value="yes"
            @click="addlike(1)"
          >
          </v-radio>
          <v-radio
            on-icon="thumb_down"
            off-icon="thumb_down"
            value="no"
            color="red"
            @click="addlike(-1)"
          ></v-radio>
        </v-radio-group>
      </div>
    </v-card-actions>
  </v-card>
</template>

<script>
export default {
  props: ["doctor", "index"],
  methods: {
    addlike(count) {
      this.$emit("liked", { index: this.index, count: count });
    },
  },
};
</script>