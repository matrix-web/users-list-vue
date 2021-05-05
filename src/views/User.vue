<template>
  <v-card class="mx-auto my-12" max-width="374">
    <v-card-title>
      {{ user.name }}
    </v-card-title>
    <v-card-text>
      <h3>Имя пользователя</h3>
      {{ user.username }}
    </v-card-text>

    <v-divider class="mx-4"></v-divider>

    <v-card-title>{{ user.email }}</v-card-title>
    <v-card-text>
      <h4>Адрес</h4>
      <p>Город: {{ user.address.city }}</p>
      <p v-if="user.address.street">Улица: {{ user.address.street }}</p>
      <p v-if="user.address.suite">Дом: {{ user.address.suite }}</p>
      <p v-if="user.address.zipcode">zipcode: {{ user.address.zipcode }}</p>
      <p v-if="user.address.lat && user.address.lng">
        geo: {{ user.address.geo.lat }}, {{ user.address.geo.lng }}
      </p>
    </v-card-text>
    <v-card-text>
      <h4>Компания</h4>
      <p>{{ user.company.name }}</p>
    </v-card-text>
    <v-card-text>
      <h4>Телефон</h4>
      <p>{{ user.phone }}</p>
    </v-card-text>
    <v-card-text>
      <h4>Веб-сайт</h4>
      <p>{{ user.website }}</p>
    </v-card-text>
  </v-card>
</template>

<script>
export default {
  name: "user",
  data: () => ({
    users: [],
    user: {},
  }),
  created() {
    this.users = JSON.parse(localStorage.getItem("users")) || [];
    const index = this.users.findIndex(
      (user) => `${user.id}` === this.$route.params.id
    );
    this.user = this.users[index];
  },
};
</script>
