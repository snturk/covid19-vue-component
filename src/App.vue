<template>
  <div id="app">
    <div id="mainContainer" v-if="covidData">
      <div id="mainTitle">COVID-19 LIVE DATA</div>
      <hr>
      <input type="text" name="country" id="countryInput" v-model="country" placeholder="search a country" style="text-align: center">
      <div id="dataContainer">
        <div v-for="item in filteredData" class="countries" v-if="country">
          <div id="country">{{item.country}}</div>
          <div id="totalCases">Total Cases: <span class="countryData">{{item.totalCases}}</span></div>
          <div id="totalRecovered">Total Recovered: <span class="countryData">{{item.totalRecovered}}</span></div>
          <div id="totalDeaths">Total Deaths: <span class="countryData">{{item.totalDeaths}}</span></div>
          <div id="newCases">New Cases: 
            <span class="countryData" v-if="item.newCases">{{item.newCases.split('+')[1]}}</span>
            <span class="countryData" v-else>0</span>
          </div>
          <div id="newDeaths">New Deaths: 
            <span class="countryData" v-if="item.newDeaths">{{item.newDeaths.split('+')[1]}}</span>
            <span class="countryData" v-else>0</span>
          </div>
        </div>
      </div>
      <div id="author">made by <a href="http://github.com/snturk" target="_blank" >muratcan şentürk</a></div>
    </div>
    <div id="loadingContainer" v-else>
      <div id="loadingCircle"></div>
      <div id="loading">loading</div>
    </div>
    
  </div>
</template>

<script>
export default {
  data(){
    return {
      country: "",
      covidData: null,
      filteredData: [],
    }
  },
  async mounted(){
    const requestOptions = {
    method: "GET",
    hostname: "api.collectapi.com",
    port: null,
    path: "/corona/countriesData",
    headers: {
      "content-type": "application/json",
      "authorization": "apikey 0LDrJPecNtjejwaJLETWi2:2ZCRkcLxxwYrnFMMMqPZ1s"
    }
  };
  const response = await fetch("https://api.collectapi.com/corona/countriesData", requestOptions);
  const data = await response.json();
  this.covidData = data.result;
  },
  watch: {
    country: function(){
      if (this.country != "") {
        this.filteredData = [];
        this.covidData.forEach(item => {
          if(item.country.includes(this.country.toString())){
            this.filteredData.push(item);
          }
        });
      }
    }
  },
  
  
}
</script>

<style>
body{
  position: absolute;
  height: 100%;
  width: 100%;
  margin: 0;
}
#app {
  top: 0;
  left: 0;
  height: 100%;
  width: 100%;
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: black;
  display: flex;
  flex-direction: column;
  margin-top: 10px;
}

#mainContainer{
  height: 55%;
  width: 300px;
  padding: 1%;
  margin-top: 4%;
  margin: 0 auto;
  border: 1.5px solid black;
  border-radius: 10px;
  background: rgb(13, 182, 13);
  box-shadow: 0 19px 38px rgba(0,0,0,0.30), 0 15px 12px rgba(0,0,0,0.22);
}

#mainTitle{
  font-size: 130%;
}
hr{
  border: 1px solid black;
  margin-bottom: 4%;
}

#countryInput{
  padding: 1.8%;
  font-size: 100%;
  margin-bottom: 4%;
  color: rgb(65, 65, 65);
}

#dataContainer{
  overflow: auto;
  background: white;
  margin: 1%;
  height: 65%;
  border: 2px solid black;
  border-radius: 5px;
  padding: 4%;
  transition-duration: 200ms;
  background: rgb(10, 139, 10);
  box-shadow: 2px 2px 4px rgba(0,0,0,0.5), inset 5px 5px 8px rgba(0,0,0,0.5);
}

.countries{
  padding: 1%;
  border: 0.7px solid rgb(75, 75, 75);
  border-radius: 5px;
  margin-top: 2%;
  transition-duration: 200ms;
  background: white;
  color: rgb(100, 91, 80);
}

#country{
  font-size: 22px;
  color: rgb(48, 43, 39);
  margin-bottom: 4%;
}

#newCases{
  margin-top: 1%;
}

.countryData{
  color: black;
  font-weight: bold;
}

#author{
  font-size: 13px;
}
</style>
