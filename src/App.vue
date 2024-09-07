<template>
    <div :class="{ 'login-container': !isContainerVisible }">
      <div v-if="!isLoggedIn" class="login-form-container">
        <h2> 로그인 </h2>
        <form @submit.prevent="login" class="login-form">
          <label for="username"> 아이디 </label>
          <input v-model="username" type="text" id="username" required />
  
          <label for="password"> 비밀번호 </label>
          <input v-model="password" type="password" id="password" required />
  
          <button name="login" type="submit" class="login-button"> 로그인 </button>
        </form>
    </div>
    <ContainerVue v-if="isContainerVisible" @logout="handleLogout" />
    </div>
    
  </template>
  
  <script>
  import ContainerVue from './components/Container.vue';
  
  export default {
    name: 'App',
    components: {
      ContainerVue,
    },
    data() {
      return {
        isContainerVisible: false,
        isLoggedIn: false,
        username: '',
        password: '',
      };
    },
    methods: {
      login() {
        if(this.username && this.password) {
          this.isLoggedIn = true;
          this.isContainerVisible = true;
        } else {
          alert('아이디와 비밀번호를 입력하세요');
        }
      },
      handleLogout() {
        this.isLoggedIn = false;
        this.isContainerVisible = false;
        this.username = '';
        this.password = '';
      }
    }
  };
  </script>
  
  <style scoped>
  .login-container {
    display: flex;
    justify-content: center;
    align-items: flex-start;
    height: 100vh; 
    background-color: #f0f0f0;
    padding-top: 120px;
  }
  .login-form-container {
    max-width: 400px;
    width: 100%;
  }
  
  .login-form { /* 로그인 폼 스타일 */
      padding: 20px;
      border-radius: 8px;
      box-shadow: 0 0 10px rbga(0, 0, 0, 0.1);
      background-color: #ffffff;
      border: 1px solid #e0e0e0;
    }
  
  .login-form label {
      display: block;
      margin-bottom: 8px;
      color: black;
      font-size: 14px;
      font-weight: 500;
    }
    
  .login-form input {
      width: 100%;
      padding: 12px;
      margin-bottom: 16px;
      border-radius: 4px;
      border: 1px solid #ddd;
      font-size: 16px;
      box-sizing: border-box;
      transition: border-color 0.3s ease, box-shadow 0.3s ease;
    }
  
  .login-from input:focus {
      box-shadow: 0 0 8px rbga(0, 123, 255, 0.2);
      outline: none;
    }
  
  .login-button { /* 버튼 스타일 */
      display: block;
      width: 100%;
      padding: 12px;
      border: none;
      border-radius: 4px;
      background-color: rgb(129, 164, 230);
      color:white;
      font-size: 16px;
      cursor: pointer;
      transition: background-color 0.3s ease, transformm 0.2s ease;
    }
    
  .login-button:hover {
      background-color: #0056b3;
    }
  .login-button:active {
      transform: scale(0.98);
    }
  </style>