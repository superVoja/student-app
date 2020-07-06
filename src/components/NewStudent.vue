<template>
  <div id="new-student">
    <h3 class="mt-3">New Student</h3>
    <div class="submit-form">
      <div v-if="!submitted">
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
        <button class="btn btn-success mt-3" @click.prevent="addStudent">Submit</button>
        <button class="btn btn-secondary mt-3">Cancel</button>
      </div>

      <div v-if="submitted">
        <h4>You submitted successfully!</h4>
      </div>
    </div>
  </div>
</template>
<script>
import db from "./firebaseInit";
export default {
  name: "new-student",
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
  methods: {
    addStudent() {
      db.collection("students")
        .add({
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
        .then(docRef => {
          this.$router.push("/");
        })
        .catch(error => console.log(error));
    }
  }
};
</script>