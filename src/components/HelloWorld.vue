<script lang="ts">
import data from './IdList.json';
export default {
  name: 'HelloWorld',
  props: {
    msg: String
  },
  data() {
    return {
      theData: {},
      tempData: {},
      resultSet: [],
      tempResultSet: [],
      currentPage: 1,
      imgURL: 'https://media.nfsacollection.net/',
      query: 'https://api.collection.nfsa.gov.au/title/',
      // query: 'https://api.collection.nfsa.gov.au/search?limit=25&hasMedia=yes&year=1993',
      itemIds: data.items,
      idNumber: 0,
      searchString: data.items[0].itemId || '',
      backgroundSlideImg: data.items[0].URL
    }
  },
  methods: {
    fetchData() {
      // use dynamic data to modify the API call
      // in this case we use a text box which sets searchString
      // and the currentPage to allow us to loop through the paginated results
      if (this.idNumber < data.items.length) { 
        this.searchString = data.items[this.idNumber].itemId
        this.backgroundSlideImg = data.items[this.idNumber].URL;
        this.idNumber++;
        let queryString = this.query + this.searchString
      console.log('API call: ' + queryString + this.searchString)

      fetch(queryString)
 .then(response => {
 // response.json().then(res => console.log(res));
          response.json().then(
            res => {this.$data.theData = res
            console.log(this.$data.theData)}
          );
 })
 .catch(err => {
 console.error(err);
 });
 }
 else if (this.idNumber >= data.items.length) { 
  this.idNumber = 0;
  this.searchString = data.items[this.idNumber].itemId
  this.fetchData()

  
}
 } 

  }
     
};
//       fetch(queryString)
//         .then((response) => {
//           response.json().then((res) => {
//             // build a temporary object, add the data from the current page on each call of fetchData()
//             this.$data.tempData = { ...this.$data.tempData, ...res }
//             // the same as above but with just the results array instead of the whole data object
//             this.$data.tempResultSet = this.$data.tempResultSet.concat(res.results)
//             // total items from the meta object (total number of items found in the search)
//             this.$data.total = res.meta.count.total
//             // if there are items
//             if (this.$data.total > 0) {
//               // check how many pages of results @ 25 per page
//               if (this.currentPage * 25 < 500 && this.currentPage * 25 < this.$data.total) {
//                 // go to the next page
//                 this.currentPage++
//                 // call this function on itself (recursive) ! be careful, this can cause an infinite loop
//                 this.fetchData()
//               } else {
//                 this.$data.theData = this.$data.tempData
//                 this.$data.tempData = {}
//                 this.$data.resultSet = this.$data.tempResultSet
//                 this.$data.tempResultSet = []
//                 // all items loaded, reset page, ready for next query
//                 this.currentPage = 1
//                 console.log('Pages: ' + Math.ceil(this.$data.total / 25))
//                 console.log('finished')
                
//               }
//             } else {
//               console.log('no results')
//             }
//           })
//         })
//         .catch((err) => {
//           console.error(err)
//         })
//     }
//   }
// }

</script>

<template>

<div class="decadeSelectWrapper">

<button class="70sWrapper"><h1>70s</h1></button>
<button class="80sWrapper"><h1>80s</h1></button>
<button class="90sWrapper"><h1>90s</h1></button>
<button class="00sWrapper"><h1>00s</h1></button>



</div>

<div class="itemsection">
  
  <div class="search">
    <h1 class="green">{{ msg }}</h1>

    <div class="bgImage" :style="{ backgroundImage: 'linear-gradient(rgba(0, 0, 0, 0.5), rgba(0, 0, 0, 0.5)), url(' + backgroundSlideImg + ')' }">
    <div class="itemWrapper">
    

      
        <h1>{{ theData['title'] }}</h1>
        <p class='est'>{{ "EST." + theData['productionDates']?.[0]?.['fromYear'] || 'N/A' }}</p>
        <p>{{ theData['summary'] }}</p>

        <!-- <p>{{ result['name'] }}</p> -->
        <!-- check if there's any items in the preview array.  If so, put the biggest image in the view -->
        <!-- v-bind is used to update the src attribute when the data comes in -->
        <img>

  <!-- v-if="result['preview'] && result['preview'][0]"
          v-bind:src="imgURL + result['preview'][0]['filePath']"
          v-bind:alt="result['name']"
          v-bind:title="result['name']"  -->
          <button class="itemFetch" @click="fetchData"><img src="./icons/down-arrow.png" alt="buttonpng"/></button>
        </div>
  </div> 
</div>
</div>
</template>
<style scoped>

.itemSection {

  display: block;
  padding-bottom: 10em;
}

.decadeSelectWrapper{

  display: flex;
}
.decadeSelectWrapper button{

  height: 100dvh;
  background-color: white;
  border: 2px solid black;
  color: black;
  padding: 15px 32px;
  text-align: center;
  text-decoration: none;
  display: inline-block;
  font-size: 16px;
  width: 25vw;

}

.decadeSelectWrapper h1 {

  color: black;
}
.itemWrapper {

  margin: 2rem;

}


.itemFetch {

  border: none;
  cursor: pointer;
  appearance: none;
  background-color: transparent;
  z-index: 1;
  display: inline-block;
  text-align: center;
}
p{ 

  width: 37.5em;
  color: white;
}
h1 {
  color: white;
  font-weight: 500;
  font-size: 2.6rem;
  position: relative;
  top: -10px;
}

.est {

  color: white;
  padding-bottom: 3rem;
}

h3 {
  font-size: 1.2rem;
}

.bgImage {
  height: 100vh;
  width: 100%;
  background-size: cover;
  background-position: center;
  position: relative;
  top: 0;
  z-index: 0;
}

.greetings h1,
.greetings h3 {
  text-align: center;
}

@media (min-width: 1024px) {
  .greetings h1,
  .greetings h3 {
    text-align: left;
  }
}
</style>
