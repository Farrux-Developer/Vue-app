<template>
  <div>
    <input v-model="userName" type="text" placeholder="Name" />
    <input v-model="userPassword" type="password" placeholder="Password" />
    <input v-model="userEmail" type="email" placeholder="Email" />
    <button @click="sendData()">{{ isEditing ? "Save" : "Send" }}</button>
    <button @click="deleteAllUsers()">Delete all Users</button>

    <div v-if="error" class="container">
      <p className="errorText">Oops! {{ error }}</p>
    </div>

    <div v-if="error2" class="container">
      <p className="errorText">Oops! {{ error2 }}</p>
    </div>

    <div v-if="users.length <= 0">
      <p>No users registered</p>
    </div>
    <div v-else-if="users.length > 0">
      <p>{{ users.length }} users registered</p>
    </div>

    <User
      v-for="(el, index) in users"
      :key="index"
      :index="index"
      :user="el"
      :deleteUser="deleteUser"
      @edit="editUser"
    />
  </div>
</template>

<script>
import User from "./components/User.vue";

export default {
  components: {
    User,
  },
  data() {
    return {
      error2: "",
      users: [],
      userName: "",
      userPassword: "",
      userEmail: "",
      error: "",
      isEditing: false,
      editIndex: null,
    };
  },
  methods: {
    sendData() {
      if (
        this.userName === "" ||
        this.userPassword === "" ||
        this.userEmail === ""
      ) {
        this.error = "Fill all fields";
        this.error2 = "";
        return;
      }

      if (this.isEditing) {
        this.users[this.editIndex] = {
          name: this.userName,
          password: this.userPassword,
          email: this.userEmail,
        };
        this.isEditing = false;
        this.editIndex = null;
      } else {
        this.users.push({
          name: this.userName,
          password: this.userPassword,
          email: this.userEmail,
        });
      }

      this.error = "";
      this.userName = "";
      this.userPassword = "";
      this.userEmail = "";
    },

    deleteUser(index) {
      if (this.isEditing && this.editIndex === index) {
        this.isEditing = false;
        this.editIndex = null;
        this.userName = "";
        this.userPassword = "";
        this.userEmail = "";
      }
      this.users.splice(index, 1);
    },

    deleteAllUsers() {
      if (this.users.length > 0) {
        this.users = [];
        this.error2 = "";
      } else {
        this.error2 = "No users to delete";
        this.error = "";
      }
      this.isEditing = false;
      this.editIndex = null;
    },

    editUser(index) {
      this.isEditing = true;
      this.editIndex = index;

      const currentUser = this.users[index];
      this.userName = currentUser.name;
      this.userPassword = currentUser.password;
      this.userEmail = currentUser.email;
    },
  },
};
</script>

<style scoped>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

a {
  text-decoration: none;
}

li {
  list-style: none;
}

body {
  background-color: #0a0b3b;
  color: #fff;
  font-family: sans-serif;
}

.container {
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
}
</style>
