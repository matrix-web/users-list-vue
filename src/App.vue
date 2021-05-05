<template>
  <v-app>
    <v-navigation-drawer
      app
      v-model="drawer"
      :mini-variant.sync="mini"
      permanent
    >
      <v-list-item class="px-2">
        <v-list-item-title>USERSDB</v-list-item-title>

        <v-btn icon @click.stop="mini = !mini">
          <v-icon>mdi-chevron-left</v-icon>
        </v-btn>
      </v-list-item>

      <v-divider></v-divider>

      <v-list dense>
        <v-list-item v-for="item in items" :key="item.title" link>
          <v-list-item-icon>
            <v-icon>{{ item.icon }}</v-icon>
          </v-list-item-icon>

          <v-list-item-content>
            <v-list-item-title>
              <router-link class="nav-link" :to="item.path">{{
                item.title
              }}</router-link>
            </v-list-item-title>
          </v-list-item-content>
        </v-list-item>
      </v-list>
    </v-navigation-drawer>

    <!-- Sizes your content based upon application components -->
    <v-main>
      <v-container fluid>
        <router-view></router-view>
      </v-container>
    </v-main>
  </v-app>
</template>

<script>
export default {
  name: "App",
  data: () => ({
    drawer: true,
    items: [
      { title: "Главная", icon: "mdi-home-city", path: "/" },
      { title: "Добавить", icon: "mdi-account", path: "create" },
    ],
    mini: true,
  }),
};
</script>

<style>
*,
*::after,
*::before {
  box-sizing: border-box;
}

.nav-link {
  display: block;
  text-decoration: none;
  padding: 5px 0;
  font-size: 16px;
  color: rgba(0, 0, 0, 0.54) !important;
}
</style>
