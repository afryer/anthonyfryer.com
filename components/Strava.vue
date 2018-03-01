<template>
  <div v-if="apiResponse">
    <h2>Year to date stats</h2>
    {{apiResponse.ytd_run_totals}}
<!--
    <p>Runs: {{ apiResponse.ytd_run_totals.count }}</p>
    <p>Distance: {{apiResponse.ytd_run_totals.distance / 1000}}km</p>
    <p>Elevation: {{apiResponse.ytd_run_totals.elevation_gain}}m</p> -->

    <a style="display:inline-block;background-color:#FC4C02;color:#fff;padding:5px 10px 5px 30px;font-size:11px;font-family:Helvetica, Arial, sans-serif;white-space:nowrap;text-decoration:none;background-repeat:no-repeat;background-position:10px center;border-radius:3px;background-image:url('http://badges.strava.com/logo-strava-echelon.png')" href='http://strava.com/athletes/663067/badge' target="_clean">
  Follow me on
  <img src='http://badges.strava.com/logo-strava.png' alt='Strava' style='margin-left:2px;vertical-align:text-bottom' height=13 width=51 />
</a>
  </div>
</template>
<script>
  import axios from 'axios'

  export default {

    data () {
      return {
        token: '76b4ec0f6143822d5f5d33a42fc554daa5f9f82d',
        athlete: '663067',
        apiResponse: []
      }
    },

    mounted: function () {
      axios.get('https://www.strava.com/api/v3/athletes/663067/stats?access_token=76b4ec0f6143822d5f5d33a42fc554daa5f9f82d')
      .then(response => {
        this.apiResponse = response.data
      })
    },

    method: {

    }
  }
</script>
