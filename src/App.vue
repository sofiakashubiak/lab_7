<template>
  <div class="registration-container">
    <h2>Registration</h2>
    <form @submit.prevent="addPerson">
      <div class="field">
        <label for="name">Name:</label>
        <input type="text" id="name" v-model="newName" placeholder="Name" @input="validateName" :class="{ 'error-border': nameError }">
        <div class="error-message" v-if="nameError">This field can't be empty and you can't use numbers</div>
      </div>
      <div class="field">
        <label for="lastName">Patronymic:</label>
        <input type="text" id="lastName" v-model="newPatronymic" placeholder="Patronymic" @input="validatePatronymic" :class="{ 'error-border': patronymicError }">
        <div class="error-message" v-if="patronymicError">This field can't be empty and you can't use numbers</div>
      </div>
      <div class="field">
        <label for="lastName">Last Name:</label>
        <input type="text" id="lastName" v-model="newLastName" placeholder="Last Name" @input="validateLastName" :class="{ 'error-border': lastNameError }">
        <div class="error-message" v-if="lastNameError">This field can't be empty and you can't use numbers</div>
      </div>
      <div class="field">
        <label for="birthday">Birthday:</label>
        <input type="date" id="birthday" v-model="newBirthday" :max="'2016-01-01'" :min="'1923-01-01'" placeholder="Birtday" @input="validateBirthday" :class="{ 'error-border': birthdayError }">
        <div class="error-message" v-if="birthdayError">Enter a birthday</div>
      </div>
      <div class="field">
        <label for="age">Age:</label>
        <input type="number" id="age" v-model="newAge" placeholder="Age" @input="validateAge" :class="{ 'error-border': ageError }">
        <div class="error-message" v-if="ageError">{{ ageError }}</div>
      </div>
      <div class="field">
        <label for="email">Email:</label>
        <input type="email" id="email" v-model="newEmail" placeholder="Email" @input="validateEmail" :class="{ 'error-border': emailError }">
        <div class="error-message" v-if="emailError">Invalid email address. Use the format example@example.com</div>
      </div>
      <div class="field">
        <label for="sport">Kind of Sport:</label>
        <input type="text" id="sport" list="kind-sport" v-model="newSport" placeholder="Kind of sport" @input="validateSport" :class="{ 'error-border': sportError }">
        <div class="error-message" v-if="sportError">Enter a kind of sport</div>
        <datalist id="kind-sport">
          <option value="Football"></option>
          <option value="Volleyball"></option>
          <option value="Basketball"></option>
          <option value="Hockey"></option>
          <option value="Athletics"></option>
        </datalist>
      </div>
      <div class="field">
        <label for="phone">Phone:</label>
        <input type="tel" id="phone" v-model="newPhone" v-maska data-maska="+38(0##)-###-##-##" placeholder="+38(0__) -___-__-__" @input="validatePhone" :class="{ 'error-border': phoneError }">
        <div class="error-message" v-if="phoneError">Enter a phone. For example '+38(099)-123-45-67'</div>
      </div>
      <div class="gender-field">
        <label>Gender:</label>
        <input type="radio" id="male" value="Male" v-model="gender" @input="validateGender">
        <label for="male">Male</label>
        <input type="radio" id="female" value="Female" v-model="gender" @input="validateGender">
        <label for="female">Female</label>
        <input type="radio" id="other" value="Other" v-model="gender" @input="validateGender">
        <label for="other">Other</label>
        <div class="error-message" v-if="genderError">Enter a gender</div>
      </div>

      <button id="registerButton" type="submit" :disabled="isDisabled">REGISTER</button>
    </form>
    <table class="table-users-data">
      <tr>
        <th>ID</th>
        <th>Name</th>
        <th>Last Name</th>
        <th>Birthday</th>
        <th>Age</th>
        <th>Email</th>
        <th>Kind of Sport</th>
        <th>Phone</th>
        <th>Gender</th>
        <th>Checkbox</th>
      </tr>
      <tr v-for="user in users" :key="user.id">
        <th>{{ user.id }}</th>
        <th>{{ user.name }}</th>
        <th>{{ user.lastName }}</th>
        <th>{{ user.birthday }}</th>
        <th>{{ user.age }}</th>
        <th>{{ user.email }}</th>
        <th>{{ user.sport }}</th>
        <th>{{ user.phone }}</th>
        <th>{{ user.gender }}</th>
        <th><input type="checkbox" v-model="user.choice"></th>
      </tr>
    </table>
    <br>
    <button @click="deleteRows">DELETE</button><br>
    <button @click="duplicateRows">DUPLICATE</button>
  </div>
</template>

<script setup>
import { ref } from "vue";
import "/src/styles/register.css"
import { computed } from "vue";
import { vMaska } from "maska";


let id = 0;
const users = ref([]);
const newName = ref("");
const newLastName = ref("");
const newPatronymic = ref("");
const newBirthday = ref("");
const newAge = ref("");
const newEmail = ref("");
const newSport = ref("");
const newPhone = ref("");
const gender = ref("");


const nameError = ref("");
const lastNameError = ref("");
const patronymicError = ref("");
const birthdayError = ref("");
const ageError = ref("");
const emailError = ref("");
const sportError = ref("");
const phoneError = ref("");
const genderError = ref("");
const fileError = ref("");

function validateName() {
  const name = newName.value.trim();
  nameError.value = name.length < 3 || !/^[a-zA-Z-']+$/u.test(name);
}

function validatePatronymic() {
  const patronymic = newPatronymic.value.trim();
  patronymicError.value = patronymic.length < 3 || !/^[a-zA-Z-']+$/u.test(patronymic);
}

function validateLastName() {
  const lastName = newLastName.value.trim();
  lastNameError.value = lastName.length < 3 || !/^[a-zA-Z-']+$/u.test(lastName);
}

function validateBirthday() {
  birthdayError.value = !newBirthday.value;
}

function validateAge() {
  if (!newAge.value || isNaN(newAge.value) || newAge.value < 7) {
    ageError.value = "Age should be 7 or older";
  } else {
    ageError.value = "";
  }
}

function validateEmail() {
  const emailPattern = /^[a-zA-Z0-9._%+-]+@[a-zAZ0-9.-]+\.[a-zA-Z]{2,4}$/;
  emailError.value = !newEmail.value || !emailPattern.test(newEmail.value);
}

function validateSport() {
  sportError.value = !newSport.value;
}

function validatePhone() {
  const phonePattern = /^\+38\(0\d{2}\)-\d{3}-\d{2}-\d{2}$/;
  phoneError.value = !newPhone.value || !phonePattern.test(newPhone.value);
}

function validateGender() {
  genderError.value = !gender.value;
}


const isDisabled = computed(() => !!nameError.value || !!lastNameError.value || !!birthdayError.value || !!ageError.value || !!emailError.value || !!sportError.value || !!phoneError.value || !!genderError.value || !!fileError.value);

function addPerson() {
  validateName();
  validateLastName();
  validatePatronymic();
  validateBirthday();
  validateAge();
  validateEmail();
  validateSport();
  validatePhone();
  validateGender();

  if (!nameError.value && !lastNameError.value && !birthdayError.value && !ageError.value && !emailError.value && !sportError.value && !phoneError.value && !genderError.value && !fileError.value) {
    users.value.push({
      id: id++,
      name: newName.value,
      patronymic: newPatronymic.value,
      lastName: newLastName.value,
      birthday: newBirthday.value,
      age: newAge.value,
      email: newEmail.value,
      sport: newSport.value,
      phone: newPhone.value,
      gender: gender.value,
      choice: false
    });

    newName.value = "";
    newPatronymic.value = "";
    newLastName.value = "";
    newBirthday.value = "";
    newAge.value = "";
    newEmail.value = "";
    newSport.value = "";
    newPhone.value = "";
    gender.value = "";
  }
}

function deleteRows() {
  users.value = users.value.filter((user) => !user.choice);
}

function duplicateRows() {
  const copyUsers = users.value.filter((user) => user.choice);

  for (let user of copyUsers) {
    user.choice = false;
    const newUser = { ...user, id: id++ };
    users.value.push(newUser);
  }
}
</script>



