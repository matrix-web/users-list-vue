<template>
  <v-card>
    <v-card-title>
      <v-text-field
        v-model="search"
        append-icon="mdi-magnify"
        label="Поиск"
        single-line
        hide-details
      ></v-text-field>
    </v-card-title>
    <v-data-table
      :headers="headers"
      :items="users"
      :search="search"
      sort-by="calories"
      class="elevation-1"
    >
      <template v-slot:top>
        <v-toolbar flat>
          <v-toolbar-title>Список пользователей</v-toolbar-title>
          <v-divider class="mx-4" inset vertical></v-divider>
          <v-spacer></v-spacer>
          <v-dialog v-model="dialogDelete" max-width="500px">
            <v-card>
              <v-card-title class="modal-title"
                >Вы действительно хотите удалить этого
                пользователя?</v-card-title
              >
              <v-card-actions>
                <v-spacer></v-spacer>
                <v-btn color="blue darken-1" text @click="closeDelete"
                  >Отмена</v-btn
                >
                <v-btn color="blue darken-1" text @click="deleteItemConfirm"
                  >OK</v-btn
                >
                <v-spacer></v-spacer>
              </v-card-actions>
            </v-card>
          </v-dialog>
        </v-toolbar>
      </template>
      <template v-slot:item.actions="{ item }">
        <v-icon small @click="deleteItem(item)"> mdi-delete </v-icon>
      </template>
      <template v-slot:item.id="{ item }">
        <router-link :to="`user/${item.id}`">{{ item.name }}</router-link>
      </template>
    </v-data-table>
  </v-card>
</template>

<script>
export default {
  name: "Home",
  data: () => ({
    search: "",
    users: [],
    dialogDelete: false,
    editedIndex: -1,
    headers: [
      {
        text: "Link",
        align: "start",
        filterable: false,
        value: "id",
      },
      { text: "Name", value: "name" || "company" },
      { text: "Username", value: "username" },
      { text: "Email", value: "email" },
      { text: "City", value: "address.city" },
      { text: "Phone", value: "phone" },
      { text: "Website", value: "website" },
      { text: "Company", value: "company.name" },
      { text: "Actions", value: "actions", sortable: false },
    ],
  }),
  async created() {
    const response = await fetch("http://jsonplaceholder.typicode.com/users");
    const users = await response.json();

    if (!localStorage.getItem("users")) {
      localStorage.setItem("users", JSON.stringify(users));
    }
  },
  mounted() {
    this.users = JSON.parse(localStorage.getItem("users")) ?? [];
  },
  methods: {
    deleteItem(item) {
      this.editedIndex = this.users.indexOf(item);
      this.dialogDelete = true;
    },
    deleteItemConfirm() {
      this.users.splice(this.editedIndex, 1);
      localStorage.setItem("users", JSON.stringify(this.users));
      this.closeDelete();
    },
    closeDelete() {
      this.dialogDelete = false;
      this.$nextTick(() => {
        this.editedIndex = -1;
      });
    },
  },
};
</script>

<style>
.modal-title {
  font-size: 1.2rem !important;
  word-break: break-word;
}
</style>
