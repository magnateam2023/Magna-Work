<template>
    <p>
        Delivery Checker
    </p>
    <table>
        <thead>
            <!-- This tr contains all the buttons that will sort the delivery checker by the chosen metric -->
            <!-- One click for ascending order and double click for descending order -->
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
            <!-- Contains the headers for each row of the table -->
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
        </thead>
        <!-- Loops through a json file and outputs each row and the given error for that entry -->
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
    </table>
</template>
 
<script>
// Grabs the json from the file
import json from '../assets/datatest.json';
    export default{
        name: 'SortTable',
        data () {
            // Setups the variables to loop through
            return {
                myJson : json,
                resetJson : json
            }    
        },
        methods: {
            // This will sort by the vehicle id in ascending order based on the number at the end of the VEH#pilot_(number) tag
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
            // This will sort the other metrics in ascending order
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
            // This will sort by the vehicle id in descending order based on the number at the end of the VEH#pilot_(number) tag
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
            // This will sort the other metrics in descending order
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
        }
    }    
</script>
 
<style scoped>
table {
    overflow-x: auto;
    border-collapse: collapse;
    border-spacing: 0;
    width: 92%;
    border: 1px solid black;
    margin-left: auto;
    margin-right: auto;
}

thead {
    background-color: lightgray;
}

table, tr, td {
  border: 1px solid;

}

th, td {
    text-align: left;
    padding: 8px;
    border: 1px black;
}

button {
    color: #888;
    background-color: white; 
    padding: 8px;
    text-align: left;
    margin-right: 1%;
}

button:hover{
    background-color: #F0F0F0;
    
}

td {
    border: 1px black;
}

tr:nth-child(even){
    background-color: lightgray;
    color: black;
    
}

</style>