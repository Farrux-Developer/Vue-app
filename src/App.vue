<template>
  <div class="app-interface">
    <div class="studio-background"></div>
    
    <div class="physical-control-panel central-form">
      <h1 class="title">HEAVENLY GUEST REGISTER</h1>
      
      <div class="input-bezel-group">
        <input v-model="userName" type="text" placeholder="GUEST NAME" class="tactile-input" />
        
        <div class="password-input-wrapper">
          <input 
            v-model="userPassword" 
            :type="isPasswordVisible ? 'text' : 'password'" 
            placeholder="PASSWORD" 
            class="tactile-input password-input" 
          />
          <button 
            type="button" 
            class="password-toggle-btn" 
            :class="{ 'active-indicator': isPasswordVisible }"
            @click="isPasswordVisible = !isPasswordVisible"
          >
            👁️
          </button>
        </div>

        <input v-model="userEmail" type="email" placeholder="ACCESS EMAIL" class="tactile-input" />
      </div>

      <div class="tactile-button-group">
        <button @click="sendData()" class="bezel-btn btn-actionable">
          {{ isEditing ? "UPDATE GUEST" : "COMMIT REGISTRY" }}
        </button>
        <button @click="deleteAllUsers()" class="bezel-btn btn-critical">PURGE ALL</button>
      </div>

      <transition name="bezel-fade">
        <div v-if="error" class="status-panel error-status">
          <p>REGISTRY ALERT: {{ error }}</p>
        </div>
      </transition>
      <transition name="bezel-fade">
        <div v-if="error2" class="status-panel info-status">
          <p>SYSTEM LOG: {{ error2 }}</p>
        </div>
      </transition>
    </div>

    <div class="module-title-bar">
      <p v-if="users.length <= 0">NO ACTIVE REGISTRIES</p>
      <p v-else>ACTIVE SOULS: {{ users.length }}</p>
    </div>

    <div class="users-grid">
      <User
        v-for="(el, index) in users"
        :key="index"
        :index="index"
        :user="el"
        :deleteUser="deleteUser"
        @edit="editUser"
      />
    </div>
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
      isPasswordVisible: false, // Состояние видимости пароля
    };
  },
  methods: {
    sendData() {
      if (this.userName === "" || this.userPassword === "" || this.userEmail === "") {
        this.error = "All registry fields required";
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
      this.isPasswordVisible = false; // Сбрасываем глаз при отправке
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
        this.error2 = "Sanctuary is already clear";
        this.error = "";
      }
      this.isEditing = false;
      this.editIndex = null;
      this.isPasswordVisible = false;
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

<style>
body {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
  font-family: 'Roboto Mono', 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
  background-color: #050508;
  color: #fff;
  min-height: 100vh;
  overflow-x: hidden;
}
</style>

<style scoped>
.app-interface {
  display: flex;
  flex-direction: column;
  align-items: center;
  padding: 40px 20px;
  min-height: 100vh;
  position: relative;
}

.studio-background {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background-image: url('data:image/svg+xml;base64,PHN2ZyB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIHdpZHRoPSIzMiIgaGVpZ2h0PSIzMiI+PHBhdGggZD0iTTAgMGgzMnY4SDh2MjRIOXY4aDIzdjh2MzJIZzB6TTE2IDhoMTZ2OEgxNnY4SDh2MzJoOHY4aDE2djMySDB6IiBmaWxsPSIjMDcwNzA9IiBmaWxsLW9wYWNpdHk9IjAuMSIvPjwvc3ZnPg==');
  background-color: #0a0a0f;
  background-size: 32px 32px;
  box-shadow: inset 0 0 100px rgba(0, 0, 0, 0.8);
  opacity: 0.8;
  pointer-events: none;
}

.title {
  text-align: center;
  margin-bottom: 30px;
  font-size: 1.8rem;
  font-weight: bold;
  letter-spacing: 3px;
  text-transform: uppercase;
  background: linear-gradient(180deg, #d8d8e0 0%, #a0a0b0 100%);
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
  text-shadow: 2px 2px 5px rgba(0, 0, 0, 0.5);
}

.physical-control-panel {
  background: radial-gradient(circle at center, #18181f 0%, #0d0d12 100%);
  border: 2px solid #282830;
  border-radius: 12px;
  padding: 40px;
  width: 100%;
  max-width: 500px;
  box-shadow: 
    5px 5px 15px rgba(0, 0, 0, 0.6),
    -5px -5px 15px rgba(255, 255, 255, 0.05),
    inset 2px 2px 5px rgba(0, 0, 0, 0.4),
    inset -2px -2px 5px rgba(255, 255, 255, 0.05);
  position: relative;
  z-index: 1;
}

.input-bezel-group {
  display: flex;
  flex-direction: column;
  gap: 20px;
  margin-bottom: 30px;
}

.tactile-input {
  width: 100%;
  padding: 15px;
  background-color: #060609;
  border: 1px solid #1a1a20;
  border-radius: 8px;
  color: #fff;
  font-family: inherit;
  font-size: 0.95rem;
  letter-spacing: 1px;
  box-sizing: border-box;
  box-shadow: 
    inset 2px 2px 5px rgba(0, 0, 0, 0.8),
    inset -2px -2px 5px rgba(255, 255, 255, 0.02);
  transition: border-color 0.2s ease, box-shadow 0.2s ease;
}

.tactile-input::placeholder {
  color: #505060;
  text-transform: uppercase;
  font-size: 0.8rem;
}

.tactile-input:focus {
  outline: none;
  border-color: #5c84d8;
  box-shadow: 
    inset 2px 2px 5px rgba(0, 0, 0, 0.9),
    inset -2px -2px 5px rgba(255, 255, 255, 0.03),
    0 0 10px rgba(92, 132, 216, 0.3);
}

/* Стили для интеграции кнопки внутрь инпута */
.password-input-wrapper {
  position: relative;
  width: 100%;
  display: flex;
  align-items: center;
}

.password-input {
  padding-right: 55px; /* Освобождаем место справа для кнопки */
}

.password-toggle-btn {
  position: absolute;
  right: 6px;
  height: calc(100% - 12px);
  width: 42px;
  background: linear-gradient(180deg, #20202a 0%, #111118 100%);
  border: 1px solid #2d2d38;
  border-radius: 6px;
  cursor: pointer;
  display: flex;
  align-items: center;
  justify-content: center;
  font-size: 1.1rem;
  /* Эффект выпирающей железной кнопки */
  box-shadow: 
    2px 2px 4px rgba(0, 0, 0, 0.5),
    inset 1px 1px 1px rgba(255, 255, 255, 0.05);
  transition: all 0.2s ease;
  user-select: none;
}

.password-toggle-btn:active {
  transform: scale(0.96);
}

/* Эффект "Включено" — кнопка проваливается и загорается неоном */
.password-toggle-btn.active-indicator {
  border-color: #5c84d8;
  background: linear-gradient(180deg, #151d2a 0%, #0b0e14 100%);
  box-shadow: 
    inset 2px 2px 4px rgba(0, 0, 0, 0.7),
    0 0 12px rgba(92, 132, 216, 0.5);
  text-shadow: 0 0 8px rgba(92, 132, 216, 0.8);
}

.tactile-button-group {
  display: flex;
  gap: 15px;
  margin-bottom: 25px;
}

.bezel-btn {
  flex: 1;
  padding: 12px 20px;
  border: none;
  border-radius: 6px;
  font-weight: bold;
  font-family: inherit;
  font-size: 0.9rem;
  cursor: pointer;
  text-transform: uppercase;
  letter-spacing: 2px;
  position: relative;
  overflow: hidden;
  box-shadow: 
    4px 4px 8px rgba(0, 0, 0, 0.6),
    -4px -4px 8px rgba(255, 255, 255, 0.05),
    inset 1px 1px 2px rgba(255, 255, 255, 0.1),
    inset -1px -1px 2px rgba(0, 0, 0, 0.4);
  transition: all 0.2s cubic-bezier(0.17, 0.88, 0.32, 1.28);
}

.bezel-btn:active {
  transform: translateY(2px) scale(0.98);
  box-shadow: 
    2px 2px 5px rgba(0, 0, 0, 0.8),
    -2px -2px 5px rgba(255, 255, 255, 0.01),
    inset 2px 2px 4px rgba(0, 0, 0, 0.6),
    inset -2px -2px 4px rgba(255, 255, 255, 0.05);
}

.btn-actionable {
  background: linear-gradient(135deg, #1a1a25 0%, #0a0a10 100%);
  border: 1px solid #3c548c;
  color: #8fb1f3;
}

.btn-actionable:hover {
  background: linear-gradient(135deg, #1f1f2e 0%, #0e0e16 100%);
  border-color: #5c84d8;
  color: #fff;
}

.btn-critical {
  background: linear-gradient(135deg, #251a1a 0%, #100a0a 100%);
  border: 1px solid #8c3c3c;
  color: #f38f8f;
}

.btn-critical:hover {
  background: linear-gradient(135deg, #2e1f1f 0%, #160e0e 100%);
  border-color: #d85c5c;
  color: #fff;
}

.status-panel {
  padding: 15px;
  border-radius: 6px;
  text-align: center;
  margin-top: 20px;
  font-family: inherit;
  font-size: 0.85rem;
  letter-spacing: 1px;
  text-transform: uppercase;
  box-shadow: 
    inset 2px 2px 5px rgba(0, 0, 0, 0.9),
    inset -2px -2px 5px rgba(255, 255, 255, 0.02);
}

.error-status {
  background: rgba(132, 10, 10, 0.2);
  border: 1px solid #6c2a2a;
  color: #f3a0a0;
}

.info-status {
  background: rgba(10, 10, 132, 0.2);
  border: 1px solid #2a2a6c;
  color: #a0a0f3;
}

.module-title-bar {
  margin: 40px 0;
  font-size: 1rem;
  color: #505060;
  letter-spacing: 4px;
  text-transform: uppercase;
  border-bottom: 1px solid #1a1a20;
  padding-bottom: 15px;
  width: 100%;
  max-width: 1000px;
  text-align: center;
}

.users-grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(320px, 1fr));
  gap: 25px;
  width: 100%;
  max-width: 1100px;
  position: relative;
  z-index: 1;
}
</style>