# Dashboards for Data Visualization
The main motivation of this project is to provide Magna engineers the ability to have dashboards that display, in real time, certain metrics on their fleet of autonomous delivery vehicles. Data on the fleet is pulled from the AWS S3 database into MySQL with a Python lambda function in AWS, then using Amazon QuickSight, this data is then visualized on several dashboards. The dashboards are seperated out by different timeframes (Daily, Weekly, Monthly, Lifetime). The dashboards are grouped into two categories: Deliveries or Emergencies. In addition to this there is also a delivery checker that will look through a json of deliveries and look for metrics that are out of bounds of certain metrics.

# Table of Contents
- [App.vue](#appvue)
- [Dashboards](#dashboards)
- [Delivery Checker](#delivery-checker)

## Project setup
In order to run this app you need to have Vue.js installed on your machine, for guidance on how to do that refer here: https://vuejs.org/guide/quick-start.html 
```
npm install
```

### Compiles and hot-reloads for development
```
npm run serve
```

### Compiles and minifies for production
```
npm run build
```

### Lints and fixes files
```
npm run lint
```
# App.vue
The app.vue file contains the navigation and routing code for the project and the css for the top navbar.
To add more options to the top navbar first you want to add the name of the page you want to import to this list of pages at the top of the app.vue page

``` javascript
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
```

The names are taken from this block of code in each of the Vue files (dailyDash for example): 
``` javascript
export default {
    name: 'dailyDash',
    props: {
      msg: String
    },
}
```

After that we added the route name to a routes data object below the imports:
``` javascript
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
  '/login' : loginPage
}
```

The routing is handled by this block of code (Taken from https://vuejs.org/guide/scaling-up/routing.html#simple-routing-from-scratch):
``` javascript
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
```

After all this we gave the ```<a>```'s in the ```<nav>``` element an href that corresponds to the route name:
``` html
<a href="#/">Daily</a>
<a href="#/weeklyDash">Weekly</a>
<a href="#/monthlyDash">Monthly</a>
<a href="#/lifetimeDash">Lifetime</a>
```  
# Dashboards
Most of the other files in the project contain pages that have the individual dashboards embedded into them. The dashboards are embedded using the [amazon-quicksight-embedding-sdk](https://github.com/awslabs/amazon-quicksight-embedding-sdk). 

You can install this sdk with the command:
```
npm install amazon-embedding-sdk
```

To replicate this for any other dashboard that you would want to add you'll want to follow these steps. 
  1. Start out with an empty ```<div>``` that will house your dashboard:
  ``` html
  <div id="test"></div>
  ```
  2. Next you will want to specify a variable in the data section of your Vue file that will contain the actaul dashbaord along with another variable that will have the required sdk called:
  ``` javascript
  data() {
      return {
        dashboard: 0,
        QuickSightEmbedding : require("amazon-quicksight-embedding-sdk")
      }
    },
  ```
  3. Lastly you want to add have something similar to this in the mounted section of the Vue file:
  ``` javascript
  mounted() {
        var containerDiv = document.getElementById("test");
        var options = {
          url: "https://us-east-2.quicksight.aws.amazon.com/sn/embed/share/accounts/589749046057/dashboards/6d99f06a-eb49-48c5-9988-2d5c8f897e9b?directory_alias=magnaproject",  
          container: containerDiv,
          scrolling: "no",
          height: "1000px",
          width: "92%",
          footerPaddingEnabled:true,
          printEnabled: true
        }
        this.dashboard = this.QuickSightEmbedding.embedDashboard(options)
      }
  ```
  The options in the options data object can vary based on what you want, for example if you want print to pdf enabled you'll want to have the option ```printEnabled``` set to true. Other options can be found [here](https://github.com/awslabs/amazon-quicksight-embedding-sdk).
  # Delivery Checker
  The delivery checker was added in order to see what deliveries had metrics that fell out of certain bounds. For instance, a delivery that travels a short distance but consumes a higher battery than what would be expected would be flagged and added to the checker. The checker in its current state reads off of a json file that is included in this project (Note that the data in this json file was generated from a python file that will also be included in the deliverables for this project). The end user will be able to sort by various metrics in this checker:
  1. Vehicle ID
  2. Start Date
  3. End Date
  4. Start Time
  5. End Time
  6. Trip Time (In Minutes)
  7. Distance Travelled (In Miles)
  7. Average Speed (MPH)
  9. Battery Consumed (%)
  10. If an Emergency Stop Occured
  11. If an Emergency Report was Made
  
  The table is broken up into 3 sections:
  1. Sorting Buttons:
  ``` html
  <tr>
    <th><button @click="sortVehicle" @dblclick="sortVehicleReverse"><img src="../assets/number.png" width="30"></button></th> 
    <th><button @click="sortGeneric(1)" @dblclick="sortGenericReverse(1)"><img src="../assets/calendar2.jpg" width="30"></button></th>
    <th><button @click="sortGeneric(2)" @dblclick="sortGenericReverse(2)"><img src="../assets/calendar2.jpg" width="30"></button></th>
    <th><button @click="sortGeneric(3)" @dblclick="sortGenericReverse(3)"><img src="../assets/clock.jpg" width="30"></button></th>
    <th><button @click="sortGeneric(4)" @dblclick="sortGenericReverse(4)"><img src="../assets/clock.jpg" width="30"></button></th>
    <th><button @click="sortGeneric(5)" @dblclick="sortGenericReverse(5)"><img src="../assets/clock.jpg" width="30"></button></th>
    <th><button @click="sortGeneric(6)" @dblclick="sortGenericReverse(6)"><img src="../assets/distance.png" width="30"></button></th>
    <th><button @click="sortGeneric(7)" @dblclick="sortGenericReverse(7)"><img src="../assets/spedometer.png" width="30"></button></th>
    <th><button @click="sortGeneric(8)" @dblclick="sortGenericReverse(8)"><img src="../assets/battery.jpg" width="30"></button></th>
    <th><button @click="sortGeneric(9)" @dblclick="sortGenericReverse(9)"><img src="../assets/stopsign.png" width="30"></button></th>
    <th><button @click="sortGeneric(10)" @dblclick="sortGenericReverse(10)"><img src="../assets/report.png" width="30"></button></th>
  </tr>
  ```
  2. Column Headers:
  ``` html
  <tr>
    <th :sortKey="vID" id="test" scope="col">Vehicle ID</th>
    <th sortKey="startDate" scope="col">Start Date</th>
    <th sortKey="endDate" scope="col">End Date</th>
    <th sortKey="startTime" scope="col">Start Time</th>
    <th sortKey="endTime" scope="col">End Time</th>
    <th sortKey="tripTime" scope="col">Trip Time (min)</th>
    <th sortKey="dist" scope="col">Distance Travelled (miles)</th>
    <th sortKey="speed" scope="col">Average Speed (mph)</th>
    <th sortKey="battery" scope="col">Battery Consumption Percentage</th>
    <th sortKey="eStop" scope="col">Emergency Stop Occurred</th>
    <th sortKey="eReport" scope="col">Emergency Report Occurred</th>
  </tr>
  ```
  3. The ```v-for``` loop that outputs the row with the corresponding data, and the error that prompted it to be in the checker:
  ``` html
  <tbody v-for="item in myJson" v-bind:key="item">
        <tr>
            <!-- The information for the vehicle -->
            <td v-for="val in item.data" v-bind:key="val">
                {{val}}
            </td>
        </tr>
        <tr>
            <!-- The error that causes the vehicle to be in this checker -->
            <td v-for="val in item.errors" v-bind:key="val">
                {{val}}
            </td>
        </tr>
  </tbody>
  ```

  There are four functions used for sorting the table two for sorting by vehicle for ascending/descending order, and two for sorting for the other metrics in either ascending/descending order.
  
  * Vehicle ID (Ascending):
  ``` javascript
  sortVehicle() {
    for (let i = 1; i < this.myJson.length; i++) {
        for (let j = 0; j < i; j++) {
            // checks the numbers from the vehicle name and compares to the other
            if (parseInt(this.myJson[i].data[0].slice(10)) < parseInt(this.myJson[j].data[0].slice(10))) {
                // If one is smaller than the other then the data at that index is set to the smaller value
                var x = this.myJson[i];
                this.myJson[i] = this.myJson[j];
                this.myJson[j] = x;
            }
        }
    }
    var color = document.getElementsByTagName('button');
    for (let i = 0; i < color.length; i++) {
        color[i].style.color = "#888";
    }
    color[0].style.color = "white";
  },
  ```

  * Vehicle ID (Descending):
  ``` javascript
  sortVehicleReverse(){
    for (let i = 1; i < this.myJson.length; i++) {
        for (let j = 0; j < i; j++) {
            // checks the numbers from the vehicle name and compares to the other
            if (parseInt(this.myJson[i].data[0].slice(10)) > parseInt(this.myJson[j].data[0].slice(10))) {
                // If one is smaller than the other then the data at that index is set to the larger value
                var x = this.myJson[i];
                this.myJson[i] = this.myJson[j];
                this.myJson[j] = x;
            }
        }
    }
    var color = document.getElementsByTagName('button');
    for (let i = 0; i < color.length; i++) {
        color[i].style.color = "#888";
    }
    color[0].style.color = "white";
  },
  ```

  * Generic (Ascending):
  ``` javascript
  sortGeneric(ind) {
    for (let i = 1; i < this.myJson.length; i++) {
        for (let j = 0; j < i; j++) {
            // If one is smaller than the other then the data at that index is set to the smaller value
            if (this.myJson[i].data[ind] < this.myJson[j].data[ind]) {
                var x = this.myJson[i];
                this.myJson[i] = this.myJson[j];
                this.myJson[j] = x;
            }
        }
    }
    var color = document.getElementsByTagName('button');
    for (let i = 0; i < color.length; i++) {
        color[i].style.color = "#888";
    }
    color[ind].style.color = "white";
  },
  ```

  * Generic (Descending):
  ``` javascript
  sortGenericReverse(ind) {
    for (let i = 1; i < this.myJson.length; i++) {
        for (let j = 0; j < i; j++) {
            // If one is larger than the other then the data at that index is set to the larger value
            if (this.myJson[i].data[ind] > this.myJson[j].data[ind]) {
                var x = this.myJson[i];
                this.myJson[i] = this.myJson[j];
                this.myJson[j] = x;
            }
        }
    }
    var color = document.getElementsByTagName('button');
    for (let i = 0; i < color.length; i++) {
        color[i].style.color = "#888";
    }
    color[ind].style.color = "white";
  }
  ```

<!-- 
### Customize configuration
See [Configuration Reference](https://cli.vuejs.org/config/). -->
