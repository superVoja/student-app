<template>
  <div id="edit-student">
    <h3 class="mt-3">Edit Student</h3>
    <div class="submit-form">
      <div class="form-group">
        <label for="id">Student ID</label>
        <input type="text" class="form-control" required name="id" v-model="student.student_id" />
      </div>
      <div class="form-group">
        <label for="name">Name</label>
        <input
          type="text"
          class="form-control"
          id="name"
          required
          v-model="student.name"
          name="name"
        />
      </div>

      <div class="form-group">
        <label for="email">Email</label>
        <input class="form-control" id="email" required v-model="student.email" name="email" />
      </div>
      <div class="form-group">
        <label for="age">Age</label>
        <input
          class="form-control"
          id="age"
          required
          v-model="student.age"
          name="age"
          type="number"
        />
      </div>
      <div class="form-group">
        <label for="phone">Phone</label>
        <input
          class="form-control"
          id="phone"
          required
          v-model="student.phone"
          name="phone"
          type="text"
        />
      </div>
      <div class="form-check form-check-inline">
        <input
          class="form-check-input"
          type="radio"
          name="communicationOption"
          id="optionOne"
          value="email"
          v-model="student.communication"
        />
        <label class="form-check-label" for="inlineRadio1">Email</label>
      </div>
      <div class="form-check form-check-inline">
        <input
          class="form-check-input"
          type="radio"
          name="communicationOption"
          id="optionTwo"
          value="phone"
          v-model="student.communication"
        />
        <label class="form-check-label" for="inlineRadio2">Phone</label>
      </div>
      <div class="form-group mt-4">
        <label for="EnglishLevel">English Level</label>
        <select
          multiple
          class="form-control"
          v-model="student.englishLevel"
          required
          id="englishLevel"
        >
          <option>1</option>
          <option>2</option>
          <option>3</option>
          <option>4</option>
          <option>5</option>
        </select>
      </div>
      <div class="form-group">
        <label for="StartDate">Available to Start</label>
        <input type="date" class="form-control" id="date" v-model="student.startDate" required />
      </div>
      <div class="form-group">
        <label for="TechnicalSkills">Technical Skills and Courses</label>
        <textarea class="form-control" id="technicalSkills" v-model="student.skills" rows="3"></textarea>
      </div>
      <div class="form-group">
        <label for="PersonalPresentation">Short Personal Presentation</label>
        <textarea
          class="form-control"
          id="personalPresentations"
          rows="3"
          v-model="student.presentation"
          placeholder="e.g. reason for joining the program"
        ></textarea>
      </div>
      <div class="form-check">
        <input
          class="form-check-input"
          type="checkbox"
          value
          id="fromHome"
          v-model="student.fromHome"
        />
        <label class="form-check-label" for="StudyFromHome">Study from Home</label>
      </div>
      <button class="btn btn-success mt-3" @click.prevent="updateStudent">Submit</button>
      <button class="btn btn-secondary mt-3">Cancel</button>

      <div v-if="submitted">
        <h4>You submitted successfully!</h4>
      </div>
    </div>
  </div>
</template>
<script>
import db from "./firebaseInit";
export default {
  name: "edit-student",
  data() {
    return {
      student: {
        student_id: null,
        name: null,
        email: null,
        age: null,
        phone: null,
        communication: null,
        englishLevel: [],
        startDate: null,
        skills: null,
        presentation: null,
        fromHome: false
      },
      submitted: false
    };
  },
  beforeRouteEnter(to, from, next) {
    db.collection("students")
      .where("student_id", "==", to.params.student_id)
      .get()
      .then(querySnapshot => {
        querySnapshot.forEach(doc => {
          next(vm => {
            vm.student.student_id = doc.data().student_id;
            vm.student.name = doc.data().name;
            vm.student.email = doc.data().email;
            vm.student.age = doc.data().age;
            vm.student.phone = doc.data().phone;
            vm.student.communication = doc.data().communication;
            vm.student.englishLevel = doc.data().englishLevel;
            vm.student.startDate = doc.data().startDate;
            vm.student.skills = doc.data().skills;
            vm.student.presentation = doc.data().presentation;
            vm.student.fromHome = doc.data().fromHome;
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
    updateStudent() {
      db.collection("students")
        .where("student_id", "==", this.$route.params.student_id)
        .get()
        .then(querySnapshot => {
          querySnapshot.forEach(doc => {
            doc.ref
              .update({
                student_id: this.student.student_id,
                name: this.student.name,
                email: this.student.email,
                age: this.student.age,
                phone: this.student.phone,
                communication: this.student.communication,
                englishLevel: this.student.englishLevel,
                startDate: this.student.startDate,
                skills: this.student.skills,
                presentation: this.student.presentation,
                fromHome: this.student.fromHome
              })
              .then(() => {
                this.$router.push({
                  name: "view-student",
                  params: { student_id: this.student.student_id }
                });
              });
          });
        });
    }
  }
};
</script>