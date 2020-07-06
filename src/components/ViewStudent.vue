<template>
  <div id="view-student">
    <ul class="list-group">
      <li class="list-group-item">Student ID: {{student_id}}</li>
      <li class="list-group-item">Name: {{name}}</li>
      <li class="list-group-item">Email: {{email}}</li>
      <li class="list-group-item">Age: {{age}}</li>
      <li class="list-group-item">Phone Number: {{phone}}</li>
      <li class="list-group-item">Preferred Way of Communication: {{communication}}</li>
      <li class="list-group-item">English Level: {{englishLevel}}</li>
      <li class="list-group-item">Available to Start: {{startDate}}</li>
      <li class="list-group-item">Technical Skills and Courses: {{skills}}</li>
      <li class="list-group-item">Short Personal Presentation: {{presentation}}</li>
      <li class="list-group-item">Study from home: {{fromHome}}</li>
    </ul>
    <router-link to="/" class="btn btn-secondary mt-5">Back</router-link>
    <button class="btn btn-danger mt-5" @click="deleteStudent">Delete Student</button>
    <router-link
      class="btn btn-warning mt-5"
      :to="{name: 'edit-student', params: {student_id: student_id}}"
    >Edit Student</router-link>
  </div>
</template>
<script>
import db from "./firebaseInit";
export default {
  name: "view-student",
  data() {
    return {
      student_id: null,
      name: null,
      email: null,
      age: null,
      student_id: null,
      phone: null,
      communication: null,
      englishLevel: null,
      startDate: null,
      skills: null,
      presentation: null,
      fromHome: null
    };
  },
  beforeRouteEnter(to, from, next) {
    db.collection("students")
      .where("student_id", "==", to.params.student_id)
      .get()
      .then(querySnapshot => {
        querySnapshot.forEach(doc => {
          next(vm => {
            vm.student_id = doc.data().student_id;
            vm.name = doc.data().name;
            vm.email = doc.data().email;
            vm.age = doc.data().age;
            vm.phone = doc.data().phone;
            vm.communication = doc.data().communication;
            vm.englishLevel = doc.data().englishLevel;
            vm.startDate = doc.data().startDate;
            vm.skills = doc.data().skills;
            vm.presentation = doc.data().presentation;
            vm.fromHome = doc.data().fromHome;
          });
        });
      });
  },
  watch: {
    $route: "fetchData"
  },
  methods: {
    fetchData() {
      db.collection("students")
        .where("student_id", "==", this.$route.params.student_id)
        .get()
        .then(querySnapshot => {
          querySnapshot.forEach(doc => {
            this.student_id = doc.data().student_id;
            this.name = doc.data().name;
            this.email = doc.data().email;
            this.age = doc.data().age;
            this.phone = doc.data().phone;
            this.communication = doc.data().communication;
            this.englishLevel = doc.data().englishLevel;
            this.startDate = doc.data().startDate;
            this.skills = doc.data().skills;
            this.presentation = doc.data().presentation;
            this.fromHome = doc.data().fromHome;
          });
        });
    },
    deleteStudent() {
      if (confirm("Are you sure?")) {
        db.collection("students")
          .where("student_id", "==", this.$route.params.student_id)
          .get()
          .then(querySnapshot => {
            querySnapshot.forEach(doc => {
              doc.ref.delete();
              this.$router.push("/");
            });
          });
      }
    }
  }
};
</script>