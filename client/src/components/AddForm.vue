<template>
  <div class="modal">
    <div class="modal-content">
      <h1>
        {{ isEditing ? "Sửa thông tin sinh viên" : "Thêm mới sinh viên" }}
      </h1>
      <input v-model="student.name" placeholder="Tên sinh viên" />
      <input v-model="student.email" type="email" placeholder="Email" />
      <input v-model="student.address" type="text" placeholder="Địa chỉ" />
      <input v-model="student.phone" type="text" placeholder="Số điện thoại" />
      <div>
        <button @click="submitForm">
          {{ isEditing ? "Cập nhật" : "Thêm" }}
        </button>
        <button @click="cancel">Hủy</button>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, watch } from "vue";
import axios from "axios";

const props = defineProps(["studentId"]);
const emit = defineEmits(["student-added", "close"]);

const student = ref({
  name: "",
  email: "",
  address: "",
  phone: "",
});

const isEditing = ref(false);

watch(
  async () => {
    if (props.studentId) {
      const res = await axios.get(`http://localhost:3000/students/${props.studentId}`);
      student.value = res.data;
      isEditing.value = true;
    } else {
      resetForm();
    }
  }
);

const submitForm = async () => {
  if (isEditing.value) {
    await axios.put(`http://localhost:3000/students/${student.value.id}`, {
      ...student.value,
      status: true,
      create_at: new Date().toISOString().split("T")[0],
    });
    alert("Cập nhật sinh viên thành công!");
  } else {
    await axios.post("http://localhost:3000/students", {
      ...student.value,
      status: true,
      create_at: new Date().toISOString().split("T")[0],
    });
    alert("Thêm sinh viên thành công!");
  }
  emit("student-added");
  cancel();
};

const cancel = () => {
  emit("close");
  resetForm();
};

const resetForm = () => {
  student.value = {
    name: "",
    email: "",
    address: "",
    phone: "",
  };
  isEditing.value = false;
};
</script>

<style scoped>
.modal {
  display: flex;
  align-items: center;
  justify-content: center;
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background-color: rgba(0, 0, 0, 0.5);
}

.modal-content {
  background-color: #fff;
  padding: 20px;
  border-radius: 5px;
  box-shadow: 0 0 10px rgba(0, 0, 0, 0.3);
}

input {
  display: block;
  margin-bottom: 10px;
  padding: 8px;
  width: 100%;
}

button {
  margin-right: 5px;
}
</style>
