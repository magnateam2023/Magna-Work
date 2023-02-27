<template>
    <div>

      <div class="dash_header">
        <a href="#/weeklyDash" class="back_button">&laquo; Weekly</a>
        <span class="header_title">
          Monthly Deliveries
        </span>
        <a href="#/lifetimeDash" class="next_button">Lifetime &raquo;</a>
      </div>
    </div>

    <div id="test"></div>

  </template>


<script>
  //import { embedDashboard } from 'amazon-quicksight-embedding-sdk';
  export default {
    name: 'monthlyDash',
    props: {
      msg: String
    },

    data() {
      // Setsup the variables needed to have a printable dashboard via the quicksight embedding sdk
      return {
        dashboard: 0,
        QuickSightEmbedding : require("amazon-quicksight-embedding-sdk")
      }
    },

    mounted() {
        // This code grabs the div we want for the dashboard to sit inside of
        var containerDiv = document.getElementById("test");
        // Then you make a data object that contains the options and parameters for them that you want the dashboard to have
        var options = {
          url: "https://us-east-1.quicksight.aws.amazon.com/sn/accounts/175814219454/dashboards/f50b627e-265a-491a-9c33-3138d2f45851?directory_alias=magna-corp-rnd",  
          container: containerDiv,
          scrolling: "no",
          height: "1000px",
          width: "92%",
          footerPaddingEnabled:true,
          printEnabled: true
        }
        // Changes the already set dashboard variable to be the now embedded dashboard given by the options list using the embedding sdk
        this.dashboard = this.QuickSightEmbedding.embedDashboard(options)
      }
  }
    
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
  .dash_header {
    width: 92%;
    padding: 1%;
    margin: auto;
    display: flex;
    justify-content: left;
    position: relative;
  }
  .header_title {
    /* display: flex; */
    position: absolute;
    left: 50%;
    top: 50%;
    transform: translate(-50%, -50%);
  }

  a {
    text-decoration: none;
    display: inline-block;
    padding: 4px 8px;
  }

  a:hover {
    background-color: #ddd;
    color: black;
  }
  .next_button {
    background-color: black;
    color: white;
    margin-left: auto;
  }
  .back_button {
    background-color: black;
    color: white;
  }
  .nav {
    border: 2px solid white;
    background-color: white;
    color: brown;
  }  
  .hello {
    height: 100%;
    margin: 0;
  }
</style>