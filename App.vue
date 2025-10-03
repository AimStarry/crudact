<template>
  <div class="p-6">
    <h1>Student Manager (MEVN CRUD)</h1>

    <!-- Add Student Form -->
    <form @submit.prevent="addStudent">
      <input v-model="newStudent.name" placeholder="Name" required />
      <input v-model.number="newStudent.age" type="number" placeholder="Age" required />
      <input v-model="newStudent.course" placeholder="Course" required />
      <button type="submit">Add Student</button>
    </form>

    <hr />

    <!-- Student List -->
    <div v-for="student in students" :key="student._id">
      <p>
        {{ student.name }} ({{ student.age }}) - {{ student.course }}
        <button @click="editStudent(student)">Edit</button>
        <button @click="deleteStudent(student._id)">Delete</button>
      </p>
    </div>

    <!-- Update Form -->
    <div v-if="editing">
      <h3>Edit Student</h3>
      <form @submit.prevent="updateStudent">
        <input v-model="editData.name" />
        <input v-model.number="editData.age" type="number" />
        <input v-model="editData.course" />
        <button type="submit">Update</button>
      </form>
    </div>
  </div>
</template>

<script>
import axios from "axios";
export default {
  data() {
    return {
      students: [],
      newStudent: { name: "", age: null, course: "" },
      editing: false,
      editData: {},
    };
  },
  methods: {
    async fetchStudents() {
      const res = await axios.get("http://localhost:5000/students");
      this.students = res.data;
    },
    async addStudent() {
      await axios.post("http://localhost:5000/students", this.newStudent);
      this.newStudent = { name: "", age: null, course: "" };
      this.fetchStudents();
    },
    editStudent(student) {
      this.editing = true;
      this.editData = { ...student };
    },
    async updateStudent() {
      await axios.put(`http://localhost:5000/students/${this.editData._id}`, this.editData);
      this.editing = false;
      this.fetchStudents();
    },
    async deleteStudent(id) {
      await axios.delete(`http://localhost:5000/students/${id}`);
      this.fetchStudents();
    }
  },
  mounted() {
    this.fetchStudents();
  }
};
</script>


<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
