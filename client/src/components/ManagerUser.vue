<template>
  <div>
    <div>
      <h1>Quản lý sinh viên</h1>
      <button @click="toggleAddForm">Thêm mới sinh viên</button>
    </div>

    <AddForm
      v-if="isAddFormVisible"
      @student-added="getStudents"
      @close="toggleAddForm"
      :studentId="selectedStudentId"
    />

    <table>
      <thead>
        <tr>
          <th><input type="checkbox" /></th>
          <th>Tên sinh viên</th>
          <th>Email</th>
          <th>Địa chỉ</th>
          <th>Số điện thoại</th>
          <th>Lựa chọn</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="student in students" :key="student.id">
          <td><input type="checkbox" /></td>
          <td>{{ student.name }}</td>
          <td>{{ student.email }}</td>
          <td>{{ student.address }}</td>
          <td>{{ student.phone }}</td>
          <td>
            <button @click="openDeleteModal(student.id)">Xóa</button>
            <button @click="updateStudent(student.id)">Sửa</button>
          </td>
        </tr>
      </tbody>
    </table>

    <div
      style="display: flex; justify-content: space-between; align-items: center"
    >
      <p>Hiển thị 5/10 bản ghi</p>
      <div>
        <button @click="prevPage">Trang trước</button>
        <span>{{ currentPage }}/{{ totalPages }}</span>
        <button @click="nextPage">Trang sau</button>
      </div>
    </div>

    <DeleteModel
      v-if="deleteModal"
      :showModal="deleteModal"
      :studentId="deleteId"
      @delete="removeById"
      @close="deleteModal = false"
    />
  </div>
</template>

<script setup>
import { onMounted, ref } from "vue";
import axios from "axios";
import DeleteModel from "./DeleteModel.vue";
import AddForm from "./AddForm.vue";

const students = ref([]);
const deleteModal = ref(false);
const deleteId = ref(null);
const isAddFormVisible = ref(false);
const selectedStudentId = ref(null);

const getStudents = async () => {
  const res = await axios.get("http://localhost:3000/students");
  students.value = res.data;
};

const removeById = async (id) => {
  await axios.delete(`http://localhost:3000/students/${id}`);
  getStudents();
};

const openDeleteModal = (id) => {
  deleteId.value = id;
  deleteModal.value = true;
};

const toggleAddForm = () => {
  isAddFormVisible.value = !isAddFormVisible.value;
  selectedStudentId.value = null;
};

const updateStudent = (id) => {
  selectedStudentId.value = id;
  isAddFormVisible.value = true;
};

onMounted(() => {
  getStudents();
});
</script>


<style>
body {
  font-family: Arial, sans-serif;
  background-color: #f5f5f5;
  margin: 0;
  padding: 20px;
}

h1 {
  color: #333;
  margin-bottom: 20px;
}

button {
  background-color: #333;
  border: none;
  color: white;
  padding: 10px 15px;
  text-align: center;
  text-decoration: none;
  display: inline-block;
  margin: 5px 2px;
  cursor: pointer;
  border-radius: 5px;
  transition: background-color 0.3s;
}

button:hover {
  background-color: #45a049;
}

table {
  width: 100%;
  border-collapse: collapse;
  margin-top: 20px;
}

th,
td {
  border: 1px solid #ddd;
  text-align: left;
  padding: 12px;
}

th {
  background-color: #333;
  color: white;
}

tr:nth-child(even) {
  background-color: #f2f2f2;
}

tr:hover {
  background-color: #ddd;
}

tfoot {
  background-color: #f9f9f9;
  font-weight: bold;
  text-align: center;
  padding: 10px;
}

p {
  margin: 10px 0;
}

.pagination {
  display: inline-block;
}

.pagination button {
  margin: 0 5px;
}

@media (max-width: 600px) {
  button {
    width: 100%;
    margin: 5px 0;
  }

  table {
    font-size: 14px;
  }
}
</style>
