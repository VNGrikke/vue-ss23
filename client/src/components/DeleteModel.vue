<template>
  <div v-if="showModal" class="modal">
    <div class="modal-content">
      <h1>Xóa sinh viên</h1>
      <p>Bạn chắc chắn muốn xóa sinh viên này?</p>
      <div>
        <button @click="deleteStudent">Xóa</button>
        <button @click="closeModal">Hủy</button>
      </div>
    </div>
  </div>
</template>
  
  <script setup>
const props = defineProps(["showModal", "studentId"]);
const emit = defineEmits(["delete", "close"]);

const closeModal = () => {
  emit("close");
};

const deleteStudent = () => {
  emit("delete", props.studentId);
  closeModal();
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
</style>
  