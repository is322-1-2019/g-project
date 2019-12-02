<template>
  <div class="card-content">
    <nav-bar></nav-bar>
    <b-message  size="is-medium" >Welcome to Group Project IS322 <b-button class="button is-primary " type="is-dark" outlined 
            @click="isCardModalActive = true">
            Lecturer
      </b-button></b-message>
      
    <b-modal :active.sync="isCardModalActive" :width="640" scroll="keep">
            <div class="card">
                <div class="card-image">
                    <figure class="image is-4by3">
                        <img src="https://www.computing.psu.ac.th/2017/wp-content/uploads/2017/10/IMG_2204_re.jpg" alt="Image">
                    </figure>
                </div>
                <div class="card-content">
                    <div class="media">
                        <div class="media-left">
                            <figure class="image is-48x48">
                                <img src="https://www.tbs.tu.ac.th/wp-content/uploads/2019/10/%E0%B8%94%E0%B8%A3.%E0%B8%A7%E0%B8%B4%E0%B8%99%E0%B8%B1%E0%B8%A2500x500-570x570.jpg" alt="Image">
                            </figure>
                        </div>
                        <div class="media-content">
                            <p class="title is-4">Dr.Winai Nadee</p>
                            <p class="subtitle is-6">email: winai@tbs.tu.ac.th</p>
                        </div>
                    </div>
                </div>
            </div>
        </b-modal>


    <b-field label="Username" expanded>
      <b-input v-model="form.username" maxlength="30" expanded></b-input>
    </b-field>
    <b-field label="Password" expanded>
      <b-input type="password" v-model="form.password" maxlength="16" password-reveal></b-input>
    </b-field>
    <b-field v-if="$v.form.$dirty && $v.form.$invalid">
      <b-message type="is-danger" has-icon v-if="!$v.form.username.required">กรุณาใส่ชื่อผู้ใช้</b-message>
      <b-message type="is-danger" has-icon v-if="!$v.form.username.email">ชื่อผู้ใช้ต้องเป็น email</b-message>
      <b-message type="is-danger" has-icon v-if="!$v.form.password.required">กรุณาใส่รหัสผ่าน</b-message>
      <b-message type="is-danger" has-icon v-if="!$v.form.password.minLength">รหัสผ่านต้องอยู่ระหว่าง 4-16 ตัวอักษร</b-message>
    </b-field>
    <b-field class="buttons" grouped>
      <b-button type="is-primary" @click="touch">Login</b-button>
      <b-button type="is-primary" @click="reset">Reset</b-button>
    </b-field>
    <p>Dirty : {{ $v.form.$dirty }}</p>
    <p>Invalid : {{ $v.form.$invalid }}</p>
    <p>Require username: {{ $v.form.username.required }}</p>
    <p>User name minLength 8-30: {{ $v.form.username.minLength }}</p>
    <p>Require password: {{ $v.form.password.required }}</p>
    <p>Password minLength 4-16: {{ $v.form.password.minLength }}</p>
    <p>Login result : {{ this.lastestMessage }}</p>
  </div>
</template>
<script>
import NavBar from "./NavBar.vue";

import { required, minLength , email} from "vuelidate/lib/validators";
import firebase from "firebase/app";
import "firebase/auth";

import { mapGetters } from "vuex";


export default {
  components: {
    NavBar
  },
  data() {
    return {
      form: {
        username: "",
        password: ""
      },
      loginResult: {},
      isCardModalActive: false
    };
  },
  validations: {
    form: {
      username: {
        required,
        minLength: email
      },
      password: {
        required,
        minLength: minLength(4)
      }
    }
  },
  methods: {
    touch() {
      this.$v.form.$touch();
      if (this.$v.form.$invalid){
        return;
      }

      firebase.auth().signInWithEmailAndPassword(this.form.username, this.form.password)
        .then(data => {
          this.loginResult = data.user;
          this.$store.dispatch("messages/addMessage", "Log in successful.");
          this.$router.push("/ex03");
        })
        .catch(error => {
          /* eslint-disable no-console */
          console.log(error);
          this.loginResult = error;
        }
      ); 
    },

    reset() {
      this.$v.form.$reset();

      firebase.auth().signOut()
        .then(data => {
          this.loginResult = data;
          
          this.$buefy.dialog.alert({
            type: "is-primary",
            title: "Logout",
            message: "คุณได้ทำการ Log out เรียบร้อยแล้ว",
            confirmText: "ปิดหน้าต่างนี้!",
            onConfirm: this.goToBottom,

          });

          this.$store.dispatch("messages/addMessage", "Log out successful.");

        })

        .catch(error => {
          this.loginResult = error;
        }

      );
    }
  },

  computed:{
    ...mapGetters("messages", ["lastestMessage"])

  }
};
</script>