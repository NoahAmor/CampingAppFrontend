<template>
    <div class="user-profile">
      <h1>User Profile</h1>
      <form @submit.prevent="updateUser">
        <label for="username">Username:</label>
        <input id="username" v-model="user.username" type="text">
  
        <label for="email">Email:</label>
        <input id="email" v-model="user.email" type="email">
  
        <label for="password">Password:</label>
        <input id="password" v-model="user.password" type="password">
  
        <button type="submit">Update Profile</button>
      </form>
    </div>
  </template>
  
  <script>
  export default {
    data() {
      return {
        user: {
          id: null,
          username: '',
          email: '',
          password: '',
          succesMessage: ''
        }
      };
    },
    methods: {
      fetchUserData() {
        const userId = localStorage.getItem('userId');
        if (!userId) {
          alert('No user logged in');
          return;
        }
        fetch(`https://localhost:7043/api/Users/${userId}`)
          .then(response => response.json())
          .then(data => {
            this.user = { ...data, password: '' }; 
          })
          .catch(error => {
            console.error('Failed to fetch user data:', error);
            alert('Failed to load user data');
          });
      },
      updateUser() {
        fetch(`https://localhost:7043/api/Users/${this.user.id}`, {
          method: 'PUT',
          headers: {
            'Content-Type': 'application/json'
          },
          body: JSON.stringify({
            id: this.user.id,
            username: this.user.username,
            password: this.user.password,
            email: this.user.email
          })
        })
        .then(response => {
          if (!response.ok) throw new Error('Failed to update profile');
          return response.json();
        })
        .then(() => {
          alert('Profile updated successfully!');
        })
        .catch(error => {
          console.error('Error updating profile:', error);
          alert('Failed to update profile');
        });
      }
    },
    mounted() {
      this.fetchUserData();
    }
  };
  </script>
  
  <style scoped>
  .user-profile-container {
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
    min-height: 100vh; 
    background-color: transparent; 
  }
  
  form {
    width: 100%; 
    max-width: 400px; 
    padding: 20px;
    background: rgba(255, 255, 255, 0.5);
    box-shadow: 0 2px 10px rgba(0, 0, 0, 0.1);
    border-radius: 8px;
    margin: auto;
  }
  
  input[type="text"], input[type="email"], input[type="password"] {
    width: calc(100% - 20px); 
    padding: 10px;
    margin-bottom: 20px;
    background: rgba(255, 255, 255, 0.6);
    border: 1px solid #ccc;
    font-family: 'Franklin Gothic Medium', 'Arial Narrow', Arial, sans-serif;
    border-radius: 4px;
  }
  
  button {
    width: calc(100% - 0px); 
    padding: 10px 20px;
    background-color: #4CAF50;
    color: white;
    border: none;
    border-radius: 4px;
    font-family: 'Franklin Gothic Medium', 'Arial Narrow', Arial, sans-serif;
    cursor: pointer;
    transition: background-color 0.3s ease;
  }
  
  button:hover {
    background-color: #45a049;
  }
  
  h1 {
    margin-bottom: 80px; 
    color: antiquewhite;
    font-family: 'Franklin Gothic Medium', 'Arial Narrow', Arial, sans-serif;
    text-align: center;
    
  }
  input {
  text-align: center;
}
  label {
    font-family: 'Franklin Gothic Medium', 'Arial Narrow', Arial, sans-serif;
    display: block;
  text-align: center;
  margin-bottom: 5px;
  }
  </style>
  