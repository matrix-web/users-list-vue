<template>
  <div class="create-user">
    <h1 class="create-user__title">Добавление пользователя</h1>
    <v-form ref="form" @submit.prevent="createUser">
      <div class="input-field">
        <v-text-field
          v-model.trim="user.firstname"
          :class="{
            invalid: $v.user.firstname.$dirty && !$v.user.firstname.required,
          }"
          label="Имя"
        ></v-text-field>
        <span
          class="helper-text invalid"
          v-if="$v.user.firstname.$dirty && !$v.user.firstname.required"
          >Это поле является обязательным</span
        >
      </div>

      <div class="input-field">
        <v-text-field
          v-model.trim="user.username"
          :class="{
            invalid: $v.user.username.$dirty && !$v.user.username.required,
          }"
          label="Имя пользователя"
        ></v-text-field>
        <span
          class="helper-text invalid"
          v-if="$v.user.username.$dirty && !$v.user.username.required"
          >Это поле является обязательным</span
        >
      </div>

      <div class="input-field">
        <v-text-field
          v-model.trim="user.email"
          :class="{
            invalid:
              ($v.user.email.$dirty && !$v.user.email.required) ||
              ($v.user.email.$dirty && !$v.user.email.email),
          }"
          label="E-mail"
        ></v-text-field>

        <span
          class="helper-text invalid"
          v-if="$v.user.email.$dirty && !$v.user.email.required"
          >Это поле является обязательным</span
        >
        <span
          class="helper-text invalid"
          v-else-if="$v.user.email.$dirty && !$v.user.email.email"
          >Некорректный email</span
        >
      </div>

      <div class="input-field">
        <v-text-field
          v-model.trim="user.city"
          :class="{ invalid: $v.user.city.$dirty && !$v.user.city.required }"
          label="Город"
        ></v-text-field>

        <span
          class="helper-text invalid"
          v-if="$v.user.city.$dirty && !$v.user.city.required"
          >Это поле является обязательным</span
        >
      </div>

      <div class="input-field">
        <v-text-field
          v-model.trim="user.phone"
          :class="{
            invalid:
              ($v.user.phone.$dirty && !$v.user.phone.required) ||
              ($v.user.phone.$dirty && !$v.user.phone.validPhone),
          }"
          label="Телефон"
        ></v-text-field>

        <span
          class="helper-text invalid"
          v-if="$v.user.phone.$dirty && !$v.user.phone.required"
          >Это поле является обязательным</span
        >
        <span
          class="helper-text invalid"
          v-else-if="$v.user.phone.$dirty && !$v.user.phone.validPhone"
          >Некорректный формат телефона. Введите +7-XXX-XXX-XX-XX</span
        >
      </div>

      <div class="input-field">
        <v-text-field
          v-model.trim="user.website"
          :class="{
            invalid: $v.user.website.$dirty && !$v.user.website.required,
          }"
          label="Web-сайт"
        ></v-text-field>

        <span
          class="helper-text invalid"
          v-if="$v.user.website.$dirty && !$v.user.website.required"
          >Это поле является обязательным</span
        >
      </div>
      <div class="input-field">
        <v-text-field
          v-model.trim="user.company"
          :class="{
            invalid: $v.user.company.$dirty && !$v.user.company.required,
          }"
          label="Компания"
        ></v-text-field>

        <span
          class="helper-text invalid"
          v-if="$v.user.company.$dirty && !$v.user.company.required"
          >Это поле является обязательным</span
        >
      </div>
      <v-btn type="submit" color="success" class="mr-4"> Добавить </v-btn>
    </v-form>
  </div>
</template>

<script>
import { required, email } from "vuelidate/lib/validators";
import { v4 } from "uuid";
const validPhone = (value) => /^\+7-(\d{3})-\d{3}-\d{2}-\d{2}/.test(value);

export default {
  name: "Create",
  data: () => ({
    user: {
      firstname: "",
      username: "",
      email: "",
      city: "",
      phone: "",
      website: "",
      company: "",
    },
  }),
  validations: {
    user: {
      firstname: {
        required,
      },
      username: {
        required,
      },
      email: {
        required,
        email,
      },
      city: {
        required,
      },
      phone: {
        required,
        validPhone,
      },
      website: {
        required,
      },
      company: {
        required,
      },
    },
  },
  methods: {
    createUser() {
      if (this.$v.$invalid) {
        this.$v.$touch();
        return;
      }

      const newUser = {
        id: v4(),
        name: this.user.firstname,
        username: this.user.username,
        email: this.user.email,
        address: {
          city: this.user.city,
        },
        phone: this.user.phone,
        website: this.user.website,
        company: {
          name: this.user.company,
        },
      };

      const users = JSON.parse(localStorage.getItem("users"));
      users.push(newUser);

      localStorage.setItem("users", JSON.stringify(users));

      this.user = {};

      this.$router.push("/");
    },
  },
};
</script>

<style>
.create-user__title {
  text-align: center;
  margin-bottom: 2rem;
}

.invalid {
  color: #ff5252;
  caret-color: #ff5252;
}

.helper-text {
  display: inline-block;
  font-size: 12px;
  min-height: 14px;
  min-width: 1px;
  position: relative;
  margin-bottom: 15px;
}
</style>
