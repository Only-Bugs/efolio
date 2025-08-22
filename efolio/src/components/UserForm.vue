<template>
  <div class="container mt-5">
    <div class="row">
      <div class="col-md-8 offset-md-2">
        <h1 class="text-center mb-3">User Information Form / Credentials</h1>
        <form @submit.prevent="submitForm">
          <div class="mb-3">
            <label for="username" class="form-label">Username</label>
            <input
              type="text"
              class="form-control"
              id="username"
              v-model="formData.username"
              @blur="() => validateName(true)"
              @input="() => validateName(false)"
            />
            <div v-if="errors.username" class="text-danger">
              {{ errors.username }}
            </div>
          </div>

          <div class="mb-3">
            <label for="password" class="form-label">Password</label>
            <input
              type="password"
              class="form-control"
              id="password"
              v-model="formData.password"
              @blur="() => validatePassword(true)"
              @input="() => validatePassword(false)"
            />
            <div v-if="errors.password" class="text-danger">
              {{ errors.password }}
            </div>
          </div>

          <div class="form-check mb-3">
            <input
              type="checkbox"
              class="form-check-input"
              id="isAustralian"
              v-model="formData.isAustralian"
              @change="validateResident"
            />
            <label class="form-check-label" for="isAustralian"
              >Australian Resident?</label
            >
            <div v-if="errors.resident" class="text-danger">
              {{ errors.resident }}
            </div>
          </div>

          <div class="mb-3">
            <label for="reason" class="form-label">Reason For Joining</label>
            <textarea
              class="form-control"
              id="reason"
              rows="3"
              v-model="formData.reason"
              @blur="validateReason"
            ></textarea>
            <div v-if="errors.reason" class="text-danger">
              {{ errors.reason }}
            </div>
          </div>

          <div class="mb-3">
            <label for="gender" class="form-label">Gender</label>
            <select
              class="form-select"
              id="gender"
              v-model="formData.gender"
              @change="validateGender"
            >
              <option value="">Select one</option>
              <option value="female">Female</option>
              <option value="male">Male</option>
              <option value="other">Other</option>
            </select>
            <div v-if="errors.gender" class="text-danger">
              {{ errors.gender }}
            </div>
          </div>

          <button type="submit" class="btn btn-primary me-2">Submit</button>
          <button type="button" class="btn btn-secondary" @click="clearForm">
            Clear
          </button>
        </form>

        <div class="row mt-5" v-if="submittedCards.length">
          <div class="d-flex flex-wrap justify-content-start">
            <div
              v-for="(card, index) in submittedCards"
              :key="index"
              class="card m-2"
              style="width: 18rem"
            >
              <div class="card-header">User Information</div>
              <ul class="list-group list-group-flush">
                <li class="list-group-item">Username: {{ card.username }}</li>
                <li class="list-group-item">Password: {{ card.password }}</li>
                <li class="list-group-item">
                  Australian Resident: {{ card.isAustralian ? "Yes" : "No" }}
                </li>
                <li class="list-group-item">Gender: {{ card.gender }}</li>
                <li class="list-group-item">Reason: {{ card.reason }}</li>
              </ul>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref } from "vue";

const formData = ref({
  username: "",
  password: "",
  isAustralian: false,
  reason: "",
  gender: "",
});

const submittedCards = ref([]);

const errors = ref({
  username: null,
  password: null,
  resident: null,
  gender: null,
  reason: null,
});

const validateName = (blur) => {
  if (formData.value.username.length < 3) {
    if (blur) errors.value.username = "Name must be at least 3 characters";
  } else {
    errors.value.username = null;
  }
};

const validatePassword = (blur) => {
  const password = formData.value.password;
  const minLength = password.length >= 8;
  const hasUppercase = /[A-Z]/.test(password);
  const hasLowercase = /[a-z]/.test(password);
  const hasNumber = /\d/.test(password);
  const hasSpecialChar = /[!@#$%^&*(),.?":{}|<>]/.test(password);

  if (
    !minLength ||
    !hasUppercase ||
    !hasLowercase ||
    !hasNumber ||
    !hasSpecialChar
  ) {
    if (blur)
      errors.value.password =
        "Password must be 8+ chars, include upper, lower, number, special char";
  } else {
    errors.value.password = null;
  }
};

const validateResident = () => {
  errors.value.resident = formData.value.isAustralian
    ? null
    : "You must confirm residency";
};

const validateGender = () => {
  errors.value.gender = formData.value.gender ? null : "Please select a gender";
};

const validateReason = () => {
  errors.value.reason =
    formData.value.reason.trim().length > 0 ? null : "Reason cannot be empty";
};

const submitForm = () => {
  validateName(true);
  validatePassword(true);
  validateResident();
  validateGender();
  validateReason();

  if (
    !errors.value.username &&
    !errors.value.password &&
    !errors.value.resident &&
    !errors.value.gender &&
    !errors.value.reason
  ) {
    submittedCards.value.push({ ...formData.value });
    clearForm();
  }
};

const clearForm = () => {
  formData.value = {
    username: "",
    password: "",
    isAustralian: false,
    reason: "",
    gender: "",
  };
  errors.value = {
    username: null,
    password: null,
    resident: null,
    gender: null,
    reason: null,
  };
};
</script>

<style scoped>
.card {
  border: 1px solid #ccc;
  border-radius: 10px;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
}
.card-header {
  background-color: #275fda;
  color: white;
  padding: 10px;
  border-radius: 10px 10px 0 0;
}
.list-group-item {
  padding: 10px;
}
.text-danger {
  font-size: 0.9rem;
}
</style>
