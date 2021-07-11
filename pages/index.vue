<template>
  <div class="card flex-row flex-wrap">
    <div class="card-header border-0">
      <img :src="randomBeer.image_url" alt="" width="400" height="400" />
    </div>
    <div class="card-block px-2">
      <h4 class="card-title">{{ randomBeer.name }}</h4>
      <p class="card-text">{{ randomBeer.description }}</p>
      <button class="button" v-on:click="showRandomBeer">Another Beer</button>
      <button class="button" v-on:click="showNonAlcRandomBeer">
        Random non alcoholic Beer
      </button>
    </div>
    <div class="container-fluid">
      <div class="row content">
        <h4>Leave a Comment:</h4>

        <div class="form-group">
          <input type="search" id="mySearch" name="q" v-model="searchText" />
        </div>
        <div class="form-check form-check-inline">
          <input
            class="form-check-input"
            type="radio"
            name="inlineRadioOptions"
            id="inlineRadio1"
            value="option1"
          />
          <label class="form-check-label" for="inlineRadio1">1</label>
        </div>
        <div class="form-check form-check-inline">
          <input
            class="form-check-input"
            type="radio"
            name="inlineRadioOptions"
            id="inlineRadio2"
            value="option2"
          />
          <label class="form-check-label" for="inlineRadio2">2</label>
        </div>
        <button v-on:click="searchBeer">Search</button>

        <br /><br />

        <div class="row" v-for="(item, index) in searchResult" :key="index">
          <div class="col-sm-2 text-center">
            <img
              :src="item.image_url"
              class="img-circle"
              height="65"
              width="65"
              alt="Avatar"
            />
          </div>
          <div class="col-sm-10">
            <h4>{{item.name}}</h4>
            <p>
              {{item.description}}
            </p>
            <br />
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios";
export default {
  data() {
    return {
      listBeer: [],
      randomBeer: {},
      searchText: "",
      searchResult: [],
    };
  },
  created() {
    this.showRandomBeer();
  },

  //ABV <= 0.05%

  methods: {
    searchBeer() {
      try {
        axios
          .get("https://api.punkapi.com/v2/beers?beer_name=" + this.searchText)
          .then((response) => {
            console.log(response.data);
            this.searchResult = response.data
          });
      } catch (error) {
        console.log(error.message);
      }
    },
    showNonAlcRandomBeer() {
      try {
        axios
          .get(`https://api.punkapi.com/v2/beers/random`)
          .then((response) => {
            console.log(response.data[0].abv <= 0.05);
            if (response.data[0].abv <= 0.05) {
              console.log(response.data[0].abv);
              this.randomBeer = response.data[0];
            }
          });
      } catch (error) {
        console.log(error.message);
      }
    },
    showRandomBeer() {
      try {
        axios
          .get(`https://api.punkapi.com/v2/beers/random`)
          .then((response) => {
            console.log("The random beer is");
            console.log(response.data);
            this.randomBeer = response.data[0];
            console.log(this.randomBeer);
          });
      } catch (error) {
        console.log(error.message);
      }
    },
  },
};
</script>

<style scoped>
img {
  float: left;
  width: 40;
  height: 40;
}
.button {
  background-color: #4caf50;
  border: none;
  color: white;
  padding: 15px 32px;
  text-align: center;
  text-decoration: none;
  display: inline-block;
  font-size: 16px;
  margin: 4px 2px;
  cursor: pointer;
}
</style></style>
