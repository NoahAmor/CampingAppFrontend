<template>
    <div class="booking-page">
      <h1>Your Bookings</h1>
      <ul v-if="bookings.length > 0">
        <li v-for="booking in bookings" :key="booking.id">
          {{ booking.name }} - {{ booking.location }}
        </li>
      </ul>
      <div v-else class="no-bookings">
        You have no bookings yet.
      </div>
    </div>
  </template>
  
  <script>
  export default {
    name: 'BookingPage',
    data() {
      return {
        bookings: [],
      };
    },
    methods: {
      fetchBookings() {
        const userId = localStorage.getItem('userId');
        if (!userId) {
          alert("You are not logged in!");
          return;
        }
  
        fetch(`https://localhost:7043/api/Bookings/byUser/${userId}`)
          .then(response => {
            if (!response.ok) throw new Error('Failed to fetch bookings');
            return response.json();
          })
          .then(bookings => {
            this.processBookings(bookings);
          })
          .catch(error => {
            console.error('Error:', error);
            alert('Failed to fetch bookings');
          });
      },
      processBookings(bookings) {
        const spotIds = new Set();
        const promises = bookings.map(booking =>
          fetch(`https://localhost:7043/api/CampingSpots/${booking.campingSpotId}`)
            .then(response => response.json())
            .then(spot => {
              if (!spot.isAvailable && !spotIds.has(spot.id)) {
                spotIds.add(spot.id);
                return {
                  id: booking.id,
                  name: spot.name,
                  location: spot.location
                };
              }
            })
        );
  
        Promise.all(promises)
          .then(results => {
            this.bookings = results.filter(booking => booking !== undefined);
            if (this.bookings.length === 0) {
              console.log("No valid bookings found.");
            }
          });
      }
    },
    mounted() {
      this.fetchBookings();
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
  
  .no-bookings {
    text-align: center;
    color: antiquewhite;
    font-size: 18px;
    font-family: 'Franklin Gothic Medium', 'Arial Narrow', Arial, sans-serif;
  }
  </style>
  