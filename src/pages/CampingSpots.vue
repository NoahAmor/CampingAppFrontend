<template>
  <div>
    <h1>Camping Spots</h1>

    <!-- Filters -->
    <div class="filters">
      <input v-model="searchQuery" type="text" placeholder="Search by name" />
      <select v-model="selectedLocation">
        <option value="">All Locations</option>
        <option v-for="location in uniqueLocations" :key="location" :value="location">
          {{ location }}
        </option>
      </select>
      <select v-model="selectedAvailability">
        <option value="">All</option>
        <option value="true">Available</option>
        <option value="false">Booked</option>
      </select>
    </div>

    <ul>
      <li v-for="spot in filteredCampingSpots" :key="spot.id">
        {{ spot.name }} - {{ spot.location }}
        <button @click="bookSpot(spot.id)" :disabled="!spot.isAvailable"
                :class="{'booked': !spot.isAvailable}">
          {{ spot.isAvailable ? 'Book' : 'Booked' }}
        </button>
      </li>
    </ul>
  </div>
</template>

<script>
export default {
  name: 'CampingSpots',
  data() {
    return {
      campingSpots: [],
      searchQuery: '',
      selectedLocation: '',
      selectedAvailability: ''
    };
  },
  computed: {
    uniqueLocations() {
      const locations = this.campingSpots.map(spot => spot.location);
      return [...new Set(locations)];
    },
    filteredCampingSpots() {
      return this.campingSpots.filter(spot => {
        const matchesName = spot.name.toLowerCase().includes(this.searchQuery.toLowerCase());
        const matchesLocation = this.selectedLocation ? spot.location === this.selectedLocation : true;
        const matchesAvailability = this.selectedAvailability !== '' ? spot.isAvailable === (this.selectedAvailability === 'true') : true;
        return matchesName && matchesLocation && matchesAvailability;
      });
    }
  },
  methods: {
    fetchCampingSpots() {
      fetch('https://localhost:7043/api/CampingSpots')
        .then(response => response.json())
        .then(data => {
          this.campingSpots = data;
        })
        .catch(error => console.error('Error fetching camping spots:', error));
    },
    bookSpot(campingSpotId) {
      const userId = localStorage.getItem('userId'); // Get the user ID from local storage
      if (!userId) {
        alert('You are not logged in!');
        return;
      }

      console.log(`Booking spot with ID: ${campingSpotId} for user ${userId}`);
      fetch('https://localhost:7043/api/Bookings', {
        method: 'POST',
        headers: {
          'Accept': 'application/json',
          'Content-Type': 'application/json'
        },
        body: JSON.stringify({
          userId: userId, // Use the dynamically retrieved user ID
          campingSpotId: campingSpotId
        })
      })
      .then(response => {
        if (!response.ok) {
          throw new Error('Failed to book the spot');
        }
        return response.json();
      })
      .then(data => {
        console.log('Booking successful!', data);
        alert('Booking successful!');
        this.fetchCampingSpots(); // Refresh the list of spots after booking
      })
      .catch(error => {
        console.error('Error booking spot:', error);
        alert('Booking failed');
      });
    }
  },
  mounted() {
    this.fetchCampingSpots();
  }
}
</script>

<style scoped>
h1 {
  color: antiquewhite;
  text-align: center;
  font-family: 'Franklin Gothic Medium', 'Arial Narrow', Arial, sans-serif;
  padding-top: 20px;
}

.filters {
  display: flex;
  justify-content: center;
  margin-bottom: 20px;
  gap: 10px;
}

.filters input,
.filters select {
  padding: 8px;
  border: 1px solid #ccc;
  border-radius: 4px;
  font-family: 'Franklin Gothic Medium', 'Arial Narrow', Arial, sans-serif;
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
  box-shadow: 2px 2px 10px rgba(0,0,0,0.1);
  text-align: center;
  font-family: 'Franklin Gothic Medium', 'Arial Narrow', Arial, sans-serif;
}

button {
  padding: 8px 16px;
  background-color: #2ecc71;
  color: white;
  border: none;
  border-radius: 4px;
  cursor: pointer;
  margin-top: 10px;
  width: 100%;
  box-sizing: border-box;
  transition: background-color 0.3s;
  font-family: 'Franklin Gothic Medium', 'Arial Narrow', Arial, sans-serif;
}

button:hover {
  background-color: #27ae60;
}

button[disabled] {
  background-color: #95a5a6;
  cursor: default;
}
</style>

