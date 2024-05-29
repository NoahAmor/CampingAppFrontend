<template>
  <div id="app">
    <div v-if="authenticated">
      <nav class="navnav">
        <ul class="navul">
          <li class = "navli" @click="currentPage = 'CampingSpots'">Camping Spots</li>
          <li class = "navli" @click="currentPage = 'UserProfile'">User Profile</li>
          <li class = "navli" @click="currentPage = 'BookingPage'">Booking</li>
          <li class = "navli" @click="currentPage = 'OwnerPage'">Owner Page</li>
          <li class = "navli" @click="logout">Logout</li>
        </ul>
      </nav>
      <component :is="currentPage"></component>
    </div>
    <LoginPage v-else @login-success="handleLogin" @go-to-register="currentPage = 'RegisterPage'"/>
    <RegisterPage v-if="currentPage === 'RegisterPage'" @register-success="handleLogin" />
  </div>
</template>

<script>
import LoginPage from './pages/LoginPage.vue';
import RegisterPage from './pages/RegisterPage.vue';
import CampingSpots from './pages/CampingSpots.vue';
import UserProfile from './pages/UserProfile.vue';
import BookingPage from './pages/BookingPage.vue';
import OwnerPage from './pages/OwnerPage.vue';

export default {
  components: {
    LoginPage,
    RegisterPage,
    CampingSpots,
    UserProfile,
    BookingPage,
    OwnerPage
  },
  data() {
    return {
      authenticated: false,
      currentPage: null 
    };
  },
  methods: {
    handleLogin() {
      this.authenticated = true;
      this.currentPage = 'CampingSpots';  
    },
    logout() {
      this.authenticated = false;
      this.currentPage = null;  
    }
  }
};
</script>

<style>
.navnav {
  width: 100%;
  position: fixed; 
  top: 0; 
  left: 0; 
  background-color: rgba(243, 244, 246, 0.2); 
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1); 
  z-index: 1000; 
  padding: 10px 20px;
  display: flex;
  justify-content: center;
  align-items: center;
}

.navul {
  list-style: none; 
  padding: 0; 
  margin: 0; 
  display: flex; 
  width: 100%; 
  justify-content: space-around; 
}

.navli {
  cursor: pointer; 
  padding: 10px 15px; 
  color: antiquewhite;
  font-family: 'Franklin Gothic Medium', 'Arial Narrow', Arial, sans-serif;
}

body {
  padding-top: 60px; 
}

</style>
