<script>
// Imports all the dashboard pages into this main app page
import dailyDash from './components/DailyDash.vue'
import weeklyDash from './components/WeeklyDash.vue'
import monthlyDash from './components/MonthlyDash.vue'
import lifetimeDash from './components/LifetimeDash.vue'
import loginPage from './components/LoginPage.vue'
import dailyEmergency from './components/DailyEmergency.vue'
import weeklyEmergency from './components/WeeklyEmergency.vue'
import monthlyEmergency from './components/MonthlyEmergency.vue'
import lifetimeEmergency from './components/LifetimeEmergency.vue'
import deliveryChecker from './components/DeliveryCheck.vue'
import vehicleLogs from './components/VehicleLogs.vue'

// Defines a data object that will tell the route calculator where to go when given one of the options
// Concept outlined in 
// https://vuejs.org/guide/scaling-up/routing.html#simple-routing-from-scratch
const routes = {
  '/': dailyDash,
  '/weeklyDash' : weeklyDash,
  '/monthlyDash' : monthlyDash,
  '/lifetimeDash' : lifetimeDash,
  '/dailyEmergency' : dailyEmergency,
  '/weeklyEmergency' : weeklyEmergency,
  '/monthlyEmergency' : monthlyEmergency,
  '/lifetimeEmergency' : lifetimeEmergency,
  '/deliveryChecker' : deliveryChecker,
  '/vehicleLogs' : vehicleLogs,
  '/login' : loginPage
}

export default {
  data() {
    return {
      currentPath: window.location.hash
    }
  },
  computed: {
    currentView() {
      return routes[this.currentPath.slice(1) || '/'] || dailyDash
    }
  },
  mounted() {
    window.addEventListener('hashchange', () => {
		this.currentPath = window.location.hash
    })
  }
}
</script>

<template>
  <!-- Outer wrapper for navigation bar at top of the page -->
  <nav class="nav_wrapper">
    <!-- Contains the list of navlinks in the upper navbar -->
    <ul class="nav_list">
      <li class="nav_list_img" margin-right="100"><img alt="logo of Magna International" width="132" height=50 src="https://www.magna.com/ResourcePackages/HighroadFramework/dist/images/logo_Magna-white.svg"></li>   
      <div class="dropdown">
        <!-- Lists of navlinks inside the dropdown menu for deliveries -->
        <li class="nav_list_elem"><a class="nav_link" href="#/dailyDash" >Deliveries</a></li>
          <div class="drop_links">
            <a href="#/">Daily</a>
            <a href="#/weeklyDash">Weekly</a>
            <a href="#/monthlyDash">Monthly</a>
            <a href="#/lifetimeDash">Lifetime</a>
          </div>
      </div>
      <div class="dropdown">
        <!-- Lists of navlinks inside the dropdown menu for emergencies -->
        <li class="nav_list_elem"><a class="nav_link" href="#/dailyEmergency" >Emergency Stops</a></li>
          <div class="drop_links">
            <a href="#/dailyEmergency">Daily</a>
            <a href="#/weeklyEmergency">Weekly</a>
            <a href="#/monthlyEmergency">Monthly</a>
            <a href="#/lifetimeEmergency">Lifetime</a>
          </div>
      </div>
      <li class="nav_list_elem"><a class="nav_link" href="#/deliveryChecker" >Delivery Errors</a></li>
      <li class="nav_list_elem"><a class="nav_link">Vehicle Logs</a></li>
      <li class="nav_list_elem"><a class="nav_link">Logout (admin@magna.com)</a></li>
    </ul>
  </nav>
  <component :is="currentView" />
</template>

<style>
body {
  margin: 0;
}

#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  width: 100%;
}

h3 {
  z-index: 2;
}

.nav_wrapper {
  height: 8vh;
  width: 100%;
  background-color: black;
  display:flex;
  align-items: center;
  margin-right: 3%;
}


.nav_list a:hover, .nav_list a:focus{
  text-decoration: none;
  color: white;
}

.nav_list, .dropdown li {
  width: 100%;
  display: flex;
  align-items: center;
  justify-content: flex-end;
}

.nav_list_elem {
  display: flex;
}

.nav_list_img {
  padding-left: 8px;
  padding-right: 8px;
  margin-right: auto;
}


.nav_list_elem .nav_link {
  padding-left: 8px;
  padding-right: 8px;
  font-size: 1.2em;
  text-decoration: none;
  color: #888;
} 

.dropdown {
  float: left;
  overflow: hidden;
}

.drop_links {
  display: none;
  position: absolute;
  background-color: black;
  z-index: 1;
}

.drop_links a {
  float: none;
  color: #888;
  padding: 12px 16px;
  text-decoration: none;
  display: block;
  text-align: center;
  
}

.dropdown:hover .drop_links {
  display: block;
}

/* Just to make the page a little more responsive to screen size */
@media (max-width: 885px){
  .nav_list_elem .nav_link {
    font-size: 1em;
  }
  .nav_list_img {
    display: none;
  }
  .nav_list {
    padding-inline-start: 0;
    justify-content: center;
  }
} 

@media (max-width: 590px){
  .nav_list_elem .nav_link {
    font-size: 2vw;
  }
  .nav_list {
    justify-content: center;
  }
} 

</style>