<template>
  <div class="register-container">
    <h1>REGISTER</h1>
    <input v-model="username" type="text" placeholder="Username" />
    <input v-model="password" type="password" placeholder="Password" />
    <input v-model="email" type="email" placeholder="Email" />
    <button @click="register">Register</button>
  </div>
</template>

<script>
export default {
  data() {
    return {
      username: '',
      password: '',
      email: ''
    };
  },
  methods: {
    register() {
      fetch('https://localhost:7043/api/Users/register', {
        method: 'POST',
        headers: { 'Content-Type': 'application/json' },
        body: JSON.stringify({ username: this.username, password: this.password, email: this.email })
      })
      .then(response => response.json())
      .then(data => {
        if (data) {
          this.$emit('register-success');
        } else {
          alert('Registration failed');
        }
      })
      .catch(() => alert('Error registering'));
    }
  }
};
</script>

<style scoped>
.register-container {
  margin-top: -60px;
  display: flex;
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
  display: inline-block;
  font-family: 'Franklin Gothic Medium', 'Arial Narrow', Arial, sans-serif;
  border: 1px solid #ccc;
  background-color: rgba(243, 244, 246, 0.8);
  border-radius: 4px;
  box-sizing: border-box;
}

button {
  background-color: rgb(0, 140, 186, 0.5);
  color: white;
  font-family: 'Franklin Gothic Medium', 'Arial Narrow', Arial, sans-serif;
  border: none;
  cursor: pointer;
}

button:hover {
  opacity: 0.8;
}
h1 {
    color: antiquewhite;
    font-family: 'Franklin Gothic Medium', 'Arial Narrow', Arial, sans-serif;
}
</style>

