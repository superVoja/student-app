<template>
  <div id="dashboard">
    <h1>All Students</h1>
    <table class="table">
      <thead>
        <tr>
          <th scope="col">Student ID</th>
          <th scope="col">Name</th>
          <th scope="col">Email</th>

          <th scope="col"></th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="student in students" :key="student.id">
          <td>{{ student.student_id }}</td>
          <td>{{ student.name }}</td>
          <td>{{ student.email }}</td>

          <td>
            <router-link :to="{name: 'view-student', params: {student_id: student.student_id}}">
              <i class="fa fa-eye"></i>
            </router-link>
          </td>
        </tr>
      </tbody>
    </table>
  </div>
</template>
<script>
import db from "./firebaseInit";
export default {
  name: "dashboard",
  data() {
    return {
      students: []
    };
  },
  created() {
    db.collection("students")
      .orderBy("student_id")
      .get()
      .then(querySnapshot => {
        querySnapshot.forEach(doc => {
          const data = {
            id: doc.id,
            student_id: doc.data().student_id,
            name: doc.data().name,
            email: doc.data().email
          };
          this.students.push(data);
        });
      });
  }
};
</script>