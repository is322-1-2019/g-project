<template>
    <div class="container" id="maincard">
      <div class="card" id="maincard">
        <data-in v-model="patientInfo"></data-in>
        <parent-of-patient v-model="parentInfo"></parent-of-patient>
        <allergic></allergic>
        <yomrub></yomrub>
        <application v-model="allergic"></application>
        <p>{{allergic}}</p>
        <p>{{patientInfo}}</p>
        <p>{{parentInfo}}</p>
      </div>
    </div>
</template>

<script>
import dataIn from './dataIn.vue'
import parentOfPatient from './parentOfPatient.vue'
import allergic from './allergic.vue'
import yomrub from './yomrub.vue'
import application from './application.vue'

export default {
  components: {
    dataIn,
    parentOfPatient,
    allergic,
    yomrub,
    application
  },
  data() {
    return {
      allergic: false,
      patientInfo: {
        cardId: "",
        callAs: "",
        firstName: "",
        lastName: "",
        birthDay: "",
        religion: "",
        nationality: '',
        origin: "",
        gender: "",
        status: "",
        carrer: "",
        symtoms: '',
        houseNum: '',
        moo: 0,
        tambon: "",
        aumpue: "",
        province: "",
        phone: "",
        email: "",
        dadFirstName: "",
        dadLastName: "",
        momFirstName: "",
        momLastName: "",
      },
      parentInfo: {
        callAs: "",
        firstName: "",
        lastName: "",
        houseNum: "",
        province: "",
        tambon: "",
        aunpue: "",
        phoneNum: "",
        email: "",
        relation: ""
      },
      hasError: false,
    };
  }
}
</script>

<style>
#maincard {
  background-color: antiquewhite
}
</style>