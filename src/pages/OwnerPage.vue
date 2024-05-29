<template>
    <div>
      <h1>Your Camping Spots</h1>
      <ul v-if="campingSpots.length > 0">
        <li v-for="spot in campingSpots" :key="spot.id">
          {{ spot.name }} - {{ spot.location }}
        </li>
      </ul>
      <div v-else>
        <p>(You have not created any camping spots yet.)</p>
      </div>
  
      <h2>Create a New Camping Spot</h2>
      <form @submit.prevent="createCampingSpot">
        <label for="name">Name:</label>
        <input type="text" v-model="newSpot.name" id="name" required />
  
        <label for="location">Location:</label>
        <input type="text" v-model="newSpot.location" id="location" required />
  
        <button type="submit">Create</button>
      </form>
    </div>
  </template>
  
  <script>
  export default {
    name: 'OwnerPage',
    data() {
      return {
        campingSpots: [],
        newSpot: {
          name: '',
          location: '',
          isAvailable: true, 
          ownerId: null
        }
      };
    },
    methods: {
      fetchCampingSpots() {
        const userId = localStorage.getItem('userId'); 
        
        fetch(`https://localhost:7043/api/CampingSpots/byOwner/${userId}`)
          .then(response => response.json())
          .then(data => {
            
            this.campingSpots = data;
          })
          .catch(error => console.error('Error fetching camping spots:', error));
      },
      createCampingSpot() {
        const userId = localStorage.getItem('userId'); 
        this.newSpot.ownerId = userId;
       
        fetch('https://localhost:7043/api/CampingSpots', {
          method: 'POST',
          headers: {
            'Content-Type': 'application/json'
          },
          body: JSON.stringify(this.newSpot)
        })
          .then(response => response.json())
          .then(data => {
            console.log('Created camping spot:', data); // Debugging statement
            this.campingSpots.push(data);
            this.newSpot.name = '';
            this.newSpot.location = '';
            this.newSpot.isAvailable = true; // Reset to true for next spot creation
          })
          .catch(error => console.error('Error creating camping spot:', error));
      }
    },
    mounted() {
      this.fetchCampingSpots();
    }
  };
  </script>
  
  <style scoped>
h1, h2 {
  color: #ffffff;
  text-align: center;
  font-family: 'Franklin Gothic Medium', 'Arial Narrow', Arial, sans-serif;
  padding-top: 20px;
}

ul {
  list-style-type: none;
  padding: 0;
  display: flex;
  flex-wrap: wrap;
  justify-content: center;
  gap: 10px;
}

li {
  background-color: #f7f7f7;
  border: 1px solid #ccc;
  border-radius: 8px;
  width: 300px;
  padding: 10px;
  margin: 10px;
  box-shadow: 2px 2px 10px rgba(0, 0, 0, 0.1);
  text-align: center;
  font-family: 'Franklin Gothic Medium', 'Arial Narrow', Arial, sans-serif;
}

form {
  display: flex;
  flex-direction: column;
  align-items: center;
  margin-top: 20px;
  padding: 10px;
  background-color: rgb(247, 247, 247, 0.6);
  border-radius: 8px;
  box-shadow: 2px 2px 10px rgba(0, 0, 0, 0.1);
  max-width: 400px;
  margin-left: auto;
  margin-right: auto;
}

label {
  margin: 10px 0 5px;
  color: #333333;
  font-family: 'Franklin Gothic Medium', 'Arial Narrow', Arial, sans-serif;
}

input[type="text"] {
  padding: 8px;
  margin-bottom: 10px;
  width: 100%;
  background-color: rgb(247, 247, 247, 0.7);
  max-width: 300px;
  box-sizing: border-box;
  border: 1px solid #ccc;
  border-radius: 4px;
}

h2 {
    margin-top: 50px;
}
button {
  padding: 8px 16px;
  background-color: rgb(46, 204, 113, 0.7);
  color: white;
  border: none;
  border-radius: 4px;
  cursor: pointer;
  margin-top: 10px;
  width: auto;
  box-sizing: border-box;
  transition: background-color 0.3s;
  font-family: 'Franklin Gothic Medium', 'Arial Narrow', Arial, sans-serif;
}

button:hover {
  background-color: #27ae60;
}

p {
  color: antiquewhite;
  text-align: center;
  font-family: 'Franklin Gothic Medium', 'Arial Narrow', Arial, sans-serif;
  padding-top: 20px;
}

  </style>
  