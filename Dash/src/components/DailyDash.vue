<template>
  <div>
    <div class="dash_header">
      <span class="header_title">
        Daily Deliveries
      </span>
      <a href="#/weeklyDash" class="next_button">Weekly &raquo;</a>
    </div>
    
    <div id="test"></div>
    
  </div>
</template>

<script>
  //import { embedDashboard } from 'amazon-quicksight-embedding-sdk';
  export default {
    name: 'dailyDash',
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
          url: "https://us-east-1.quicksight.aws.amazon.com/sn/embed/share/accounts/064484330727/dashboards/37d63f18-0e0c-4237-9faf-42824a521d64?directory_alias=joeymeng2050",  
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

  #test {
    height: 92%
  }
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
