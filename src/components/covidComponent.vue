<template>
  <div id="container">
    <div id="mainContainer" v-if="loaded">
      <div id="mainTitle">COVID-19 LIVE DATA <div></div></div>
      <hr>
      <div id="inputContainer">
        <input type="text" v-if="!showTotal" name="country" id="countryInput" v-model="country" placeholder="search a country" style="text-align: center" autocomplete="off">
        <div id="showTotalBtn" v-on:click="filterTotal()">{{!showTotal ? 'Total' : 'Countries'}}</div>
      </div>
      <div id="dataContainer">
        <div v-if="country && !showTotal">
          <div v-for="item in filteredData" :key="item.country" class="countries">
            <div id="country">- {{item.country}} -</div>
            <div id="totalCases">Total Cases: <span class="covidData">{{item.totalCases}}</span></div>
            <div id="totalRecovered">Total Recovered: <span class="covidData">{{item.totalRecovered}}</span></div>
            <div id="totalDeaths">Total Deaths: <span class="covidData">{{item.totalDeaths}}</span></div>
            <div id="newCases">New Cases: 
              <span class="covidData" v-if="item.newCases">{{item.newCases.split('+')[1]}}</span>
              <span class="covidData" v-else>0</span>
            </div>
            <div id="newDeaths">New Deaths: 
              <span class="covidData" v-if="item.newDeaths">{{item.newDeaths.split('+')[1]}}</span>
              <span class="covidData" v-else>0</span>
            </div>
          </div>
        </div>
        <div v-if="!country && !showTotal">
          <div v-for="item in covidData" :key="item.country" class="countries">
            <div id="country">- {{item.country}} -</div>
            <div id="totalCases">Total Cases: <span class="covidData">{{item.totalCases}}</span></div>
            <div id="totalRecovered">Total Recovered: <span class="covidData">{{item.totalRecovered}}</span></div>
            <div id="totalDeaths">Total Deaths: <span class="covidData">{{item.totalDeaths}}</span></div>
            <div id="newCases">New Cases: 
              <span class="covidData" v-if="item.newCases">{{item.newCases.split('+')[1]}}</span>
              <span class="covidData" v-else>0</span>
            </div>
            <div id="newDeaths">New Deaths: 
              <span class="covidData" v-if="item.newDeaths">{{item.newDeaths.split('+')[1]}}</span>
              <span class="covidData" v-else>0</span>
            </div>
          </div>
        </div>
        <div id="totalData" v-if="showTotal">
          <div id="totalCases">Total Cases: <span class="covidData">{{totalData.totalCases}}</span></div>
          <div id="totalRecovered">Total Recovered: <span class="covidData">{{totalData.totalRecovered}}</span></div>
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
      country: '',
      showTotal: false,
      covidData: {},
      loaded: false,
      totalData: null,
      filteredData: [],
      requestOptions: {
        headers: {
          "content-type": "application/json",
          "authorization": "apikey 0LDrJPecNtjejwaJLETWi2:2ZCRkcLxxwYrnFMMMqPZ1s"
        }
      }
    }
  },
  created(){
    this.loaded = false;

    //this.retrieveCountriesData();
    this.retrieveTotalData();
    this.loaded = true;
  },
  methods: {
    filterTotal(){
      this.showTotal = !this.showTotal;
    },
    retrieveCountriesData() {
      this.loaded = false;

      console.log("Retrieving Countries Data");
      fetch("https://api.collectapi.com/corona/countriesData", this.requestOptions).then(res => {
        res.json().then(data => {
          this.covidData = data.result;
        });
      }).catch((err) => {
        console.error(err);
      }).finally(() => {
        this.loaded = true;
      });
    },
    retrieveTotalData() {
      this.loaded = false;

      console.log("Retrieving Countries Data");
      fetch("https://api.collectapi.com/corona/totalData", this.requestOptions).then(res => {
        res.json().then(data => {
          this.totalData = data.result;
        });
      }).catch((err) => {
        console.error(err);
      }).finally(() => {
        this.loaded = true;
      });
    }
      },
  watch: {
    country: function(){
      if (this.country != "") {
        
        this.filteredData = [];
        this.covidData.forEach(item => {
          var input = this.country.toString().toLowerCase();
          var data = item.country.toLowerCase();
          if(data.includes(input)){
            this.filteredData.push(item);
          }
        });
      }
    }
  },
}
</script>

<style>
#container{
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
  align-items: center;
  justify-content: center;
  flex-direction: column;
}
/* loading screen */
#loadingCircle {
  border: 7px solid rgba(0, 0, 0, 0);
  border-top: 7px solid #d61313;
  border-radius: 50%;
  width: 40px;
  height: 40px;
  margin: 0 auto;
  animation: loading 5s linear infinite;
  transition-duration: 180ms;
}
#loading{margin-top: 20px;}

@keyframes loading {
  0% { 
    transform: rotate(0deg);
    border-top: 7px solid #d61313;
    border-right: 7px solid gold;
    border-left: 7px solid greenyellow;
    border-bottom: 7px solid royalblue;
    border-radius: 10%;
  }
  6.25% { 
    transform: rotate(20deg);
    border-top: 7px solid #d61313;
    border-right: 7px solid rgba(0, 0, 0, 0);
    border-left: 7px solid rgba(0, 0, 0, 0);
    border-bottom: 7px solid rgba(0, 0, 0, 0);
    border-radius: 50%;
   }
  12.5% { 
    transform: rotate(340deg);
    border-top: 7px solid #d61313;
    border-right: 7px solid rgba(0, 0, 0, 0);
    border-left: 7px solid rgba(0, 0, 0, 0);
    border-bottom: 7px solid rgba(0, 0, 0, 0);
    border-radius: 50%;
    }
  18.75% {
    transform: rotate(360deg);
    border-top: 7px solid #d61313;
    border-right: 7px solid gold;
    border-left: 7px solid greenyellow;
    border-bottom: 7px solid royalblue;
    border-radius: 10%;
  }
  25%{
    transform: rotate(360deg);
    border-top: 7px solid #d61313;
    border-right: 7px solid gold;
    border-left: 7px solid greenyellow;
    border-bottom: 7px solid royalblue;
  }
  31.25%{
    transform: rotate(340deg);
    border-top: 7px solid rgba(0, 0, 0, 0);
    border-right: 7px solid gold;
    border-left: 7px solid rgba(0, 0, 0, 0);
    border-bottom: 7px solid rgba(0, 0, 0, 0);
    border-radius: 50%;
  }
  37.5%{
    transform: rotate(20deg);
    border-top: 7px solid rgba(0, 0, 0, 0);
    border-right: 7px solid gold;
    border-left: 7px solid rgba(0, 0, 0, 0);
    border-bottom: 7px solid rgba(0, 0, 0, 0);
    border-radius: 50%;
  }
  43.75%{
    transform: rotate(0deg);
    border-top: 7px solid #d61313;
    border-right: 7px solid gold;
    border-left: 7px solid greenyellow;
    border-bottom: 7px solid royalblue;
    border-radius: 10%;
  }
  50%{
    transform: rotate(0deg);
    border-top: 7px solid #d61313;
    border-right: 7px solid gold;
    border-left: 7px solid greenyellow;
    border-bottom: 7px solid royalblue;
    border-radius: 10%;
  }
  56.25%{
    transform: rotate(20deg);
    border-top: 7px solid rgba(0, 0, 0, 0);
    border-right: 7px solid rgba(0, 0, 0, 0);
    border-left: 7px solid rgba(0, 0, 0, 0);
    border-bottom: 7px solid royalblue;
    border-radius: 50%;
  }
  62.5%{
    transform: rotate(340deg);
    border-top: 7px solid rgba(0, 0, 0, 0);
    border-right: 7px solid rgba(0, 0, 0, 0);
    border-left: 7px solid rgba(0, 0, 0, 0);
    border-bottom: 7px solid royalblue;
    border-radius: 50%;
  }
  75%{
    transform: rotate(360deg);
    border-top: 7px solid #d61313;
    border-right: 7px solid gold;
    border-left: 7px solid greenyellow;
    border-bottom: 7px solid royalblue;
    border-radius: 10%;
  }
  81.25%{
    transform: rotate(360deg);
    border-top: 7px solid #d61313;
    border-right: 7px solid gold;
    border-left: 7px solid greenyellow;
    border-bottom: 7px solid royalblue;
    border-radius: 10%;
  }
  87.5%{
    transform: rotate(340deg);
    border-top: 7px solid rgba(0, 0, 0, 0);
    border-right: 7px solid rgba(0, 0, 0, 0);
    border-left: 7px solid greenyellow;
    border-bottom: 7px solid rgba(0, 0, 0, 0);
    border-radius: 50%;
  }
  93.75%{
    transform: rotate(20deg);
    border-top: 7px solid rgba(0, 0, 0, 0);
    border-right: 7px solid rgba(0, 0, 0, 0);
    border-left: 7px solid greenyellow;
    border-bottom: 7px solid rgba(0, 0, 0, 0);
    border-radius: 50%;
  }
  100%{
    transform: rotate(0deg);
    border-top: 7px solid #d61313;
    border-right: 7px solid gold;
    border-left: 7px solid greenyellow;
    border-bottom: 7px solid royalblue;
    border-radius: 10%;
  }

}

#mainContainer{
  height: fit-content;
  width: 300px;
  padding: 2%;
  margin-top: 4%;
  margin: 0 auto;
  border: 1.5px solid black;
  border-radius: 10px;
  background: #88d2db;
  box-shadow: 0 19px 38px rgba(0,0,0,0.30), 0 15px 12px rgba(0,0,0,0.22);
}

#mainTitle{
  font-size: 130%;
  font-family: 'Roboto Slab', serif;
  display: flex;
  justify-content: center;
}
#mainTitle div{
  background: red;
  width: 10px;
  height: 10px;
  border-radius: 50%;
  animation: blink 1.5s infinite;
}
@keyframes blink {
  from{background: none;}
  to{background: red;}

}
hr{
  border: 1px solid black;
  margin-bottom: 4%;
}

#inputContainer{
  display: flex;
  flex-direction: row;
  align-items: center;
  justify-content: center;
  font-family: 'Roboto', serif;
  margin-top: 6%;
  margin-bottom: 6%;
}

#countryInput{
  padding: 2.3%;
  font-size: 100%;
  margin-right: 2%;
  border: 1px solid black;
  color: rgb(65, 65, 65);
  transition-duration: 280ms;
}
#countryInput:focus{
  outline: none;
  border: #000;
  transform: scale(1.05);
}

#showTotalBtn{
  border: 1px solid black;
  border-radius: 7px;
  margin-left: 2%;
  padding: 2.3%;
  background: black;
  color: whitesmoke;
  cursor: pointer;
}

#dataContainer{
  position: relative;
  overflow: auto;
  background: white;
  height: 250px;
  margin: 1%;
  margin-bottom: 2%;
  border: 2px solid black;
  border-radius: 5px;
  padding: 4%;
  transition-duration: 200ms;
  background: #719da3;
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
  font-family: 'Roboto Slab', sans-serif;
  font-weight: 300;
}

#country{
  font-size: 22px;
  font-family: 'Roboto Slab', sans-serif;
  color: rgb(48, 43, 39);
  margin-bottom: 4%;
}

#newCases{
  margin-top: 1%;
}

.covidData{
  color: black;
  font-weight: bold;
  font-family: 'Roboto Slab', sans-serif;
}

#totalData{
  padding: 3%;
  border: 0.7px solid rgb(75, 75, 75);
  border-radius: 5px;
  margin-top: 3%;
  font-size: 100%;
  transition-duration: 200ms;
  background: white;
  color: rgb(100, 91, 80);
  font-family: 'Roboto Slab', sans-serif;
  font-weight: 300;
}

#totalData div:last-child{
  margin-top: 4%;
}

#author{
  font-size: 13px;
}

@media only screen and (max-width: 728px){
  #mainContainer{
    height: 90%;
  }
}
</style>