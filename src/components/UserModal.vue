<template>
<div v-if="isModalOpen" class="modal-overlay" @click.self="closeModal">
    <div class="modal-content">
      <p>Username: <strong>{{ selectedUser.username }}</strong></p>
      <label>
        Active:
        <input type="checkbox" v-model="selectedUser.isActive" />
      </label>
      <button @click="saveUser" class="center-button">OK</button>
    </div>
  </div>
</template>

<script>
export default {
    props: {
        selectedUser: {
            type: Object,
            Required: true
        },
        isModalOpen: {
            type: Boolean,
            Required: true
        },
        closeModal: {
            type: Function,
            Required: true
        },
        saveUser: {
            type: Function,
            Required: true
        }
    }
}
</script>

<style scoped>
.modal-overlay {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background: rgba(0, 0, 0, 0.5);
  display: flex;
  justify-content: center;
  align-items: center;
  z-index: 1000;
}

.modal-content {
  background: #2e5b2d;
  padding: 20px;
  border-radius: 8px;
  width: 300px;
  box-shadow: 0 4px 6px rgba(121, 106, 106, 0.99);
}

.modal-content p,
.modal-content label {
  margin-bottom: 10px;
  text-align: left;
}

.center-button {
  display: block;
  margin: 20px auto 0;
  padding: 10px 20px;
  /* background-color: #007bff;
  color: white; */
  border: none;
  border-radius: 5px;
  cursor: pointer;
}

.center-button:hover {
  background-color: #0056b3;
}
</style>