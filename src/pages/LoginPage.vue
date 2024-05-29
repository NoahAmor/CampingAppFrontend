<template>
    <div class="login-container">
      <h1>LOGIN</h1>
      <input v-model="username" type="text" placeholder="Username" />
      <input v-model="password" type="password" placeholder="Password" />
      <button @click="login">LOGIN</button>
      <p>Don't have an account? <span @click="$emit('go-to-register')">Register here</span></p>
    </div>
  </template>
  
  <script>
  export default {
  data() {
    return {
      username: '',
      password: ''
    };
  },
  methods: {
    login() {
      fetch('https://localhost:7043/api/Users/login', {
        method: 'POST',
        headers: { 'Content-Type': 'application/json' },
        body: JSON.stringify({ username: this.username, password: this.password })
      })
      .then(response => {
        if (!response.ok) {
          throw new Error('Login failed');
        }
        return response.json();
      })
      .then(data => {
        localStorage.setItem('userId', data.id); 
        this.$emit('login-success');
      })
      .catch(error => {
        console.error('Error logging in', error);
        alert('Login failed');
      });
    }
  }
};
  </script>
  
  <style scoped>
 .login-container {
  display: flex;
  margin-top: -60px;
  z-index: 1000;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  min-height: 100vh; 
  background-color: rgba(243, 244, 246, 0.05); 
  padding: 20px;
  box-shadow: 0 0 10px rgba(0,0,0,0.1);
}

input, button {
  width: 80%; 
  padding: 12px 20px;
  margin: 8px 0;
  color: rgb(34, 33, 33);
  display: inline-block;
  font-family: 'Franklin Gothic Medium', 'Arial Narrow', Arial, sans-serif;
  background-color: rgba(243, 244, 246, 0.8);
  border: 1px solid #ccc;
  border-radius: 4px;
  box-sizing: border-box;
}

button {
  background-color: rgba(76, 175, 80, 0.7); 
  color: white;
  font-family: 'Franklin Gothic Medium', 'Arial Narrow', Arial, sans-serif;
  border: none;
  cursor: pointer;
}

button:hover {
    background-color: rgba(76, 175, 80, 0.9);
}

span {
  color: #0645AD;
  cursor: pointer;
}

h1 {
    color: antiquewhite;
    font-family: 'Franklin Gothic Medium', 'Arial Narrow', Arial, sans-serif;
}

  </style>
  