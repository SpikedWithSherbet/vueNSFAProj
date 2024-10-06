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
      listData: {},
      resultSet: [],
      tempResultSet: [],
      imgURL: 'https://media.nfsacollection.net/',
      query: 'https://api.collection.nfsa.gov.au/title/',
      listQuery: 'https://api.collection.nfsa.gov.au/title-list/',
      // query: 'https://api.collection.nfsa.gov.au/search?limit=25&hasMedia=yes&year=1993',
      itemIds: data.itemsetfull.items,
      idNumber: 0,
      searchString: data.itemsetfull.items[0].itemId || '',
      backgroundSlideImg: data.itemsetfull.items[0].URL,
      currentitemset: data.itemsetfull,
      itemsPerPage: 6,
    currentPage: 1,
    totalPages: 0,
    listNames: []
    }
  },

  mounted() {
    this.fetchList();// Call fetchList when the component is mounted
    this.fetchData();  
  },

  computed: {
  paginatedItems() {
    const start = (this.currentPage - 1) * this.itemsPerPage;
    return this.itemIds.slice(start, start + this.itemsPerPage);
  },
  totalPages() {
    return Math.ceil(this.itemIds.length / this.itemsPerPage);
  }
},

  methods: {
    fetchData() {
      // use dynamic data to modify the API call
      // in this case we use a text box which sets searchString
      // and the currentPage to allow us to loop through the paginated results
      if (this.idNumber < this.currentitemset.items.length) { 
        this.searchString = this.currentitemset.items[this.idNumber].itemId
        this.backgroundSlideImg = this.currentitemset.items[this.idNumber].URL;
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
 else if (this.idNumber >= this.currentitemset.items.length) { 
  this.idNumber = 0;
  this.searchString = this.currentitemset.items[this.idNumber].itemId
  this.fetchData()

  
}
 }, 

 



 changeto70s(){ 

this.currentitemset = data.itemset70s
this.idNumber = 0
this.fetchData()

 },
 changeto80s(){ 

this.currentitemset = data.itemset80s
this.idNumber = 0
this.fetchData()

 },
 changeto90s(){ 

this.currentitemset = data.itemset90s
this.idNumber = 0
this.fetchData()

 },

 changeto00s(){ 

this.currentitemset = data.itemset00s
this.idNumber = 0
this.fetchData()

 },

// fetchList() {
//   this.listNames = [];
//   for (const item of data.itemsetfull.items) {
//     this.listQuery += `${item.itemId},`;
    
//   }

//   fetch(this.listQuery)
//  .then(response => {
//  // response.json().then(res => console.log(res));
//           response.json().then(
//             res => {this.$data.listData = res
//             }
//           )
//  })
//  .then(res => {
//       this.$data.listData = res; // Assign the fetched data to listData
//       console.log(this.$data.listData)
//       // Make sure listData is an array before iterating
//       if (Array.isArray(this.$data.listData)) {
//         this.listNames = []; // Clear existing names
//         for (const item of this.$data.listData) {
//           if (item?.['title']) {
//             this.listNames.push(item['title']);
//           }
//         }
//       } else {
//         console.error('listData is not an array:', this.listData);
//       }
//     })
//     .catch(err => {
//       console.error('Error fetching list:', err);
//     });
// }

// This next part needed some AI too. I'll walk through the steps
fetchList() {
  // appends the item IDs to the listQuery
  for (const item of data.itemsetfull.items) {
    this.listQuery += `${item.itemId},`;
  }

  fetch(this.listQuery)
    .then(response => {
      return response.json();
    })
    .then(res => {
      this.listData = res; // Turns the listData object into the results and stores it

      // Make sure listData is an array before iterating
      if (Array.isArray(this.listData)) {
        this.listNames = []; // Clears existing names so it will reset everytime the function is called
        for (const item of this.listData) { //for loop to look for every item within the data
          if (item?.['title']) { //does the item have a title?
            this.listNames.push(item['title']); //.push adds the item's title to the array
          }
        }
      } else {
        console.error('listData is not an array:', this.listData); //throws an error if listData for some reason isn't an array.
      }
    })
    .catch(err => {
      console.error('Error fetching list:', err); //if it fails to fetch the data in general
    });

    console.log(this.listNames)
}


  }
  
  

}
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

<button class="70sWrapper" @click="changeto70s"><h1>70s</h1></button>
<button class="80sWrapper" @click="changeto80s"><h1>80s</h1></button>
<button class="90sWrapper" @click="changeto90s"><h1>90s</h1></button>
<button class="00sWrapper" @click="changeto00s"><h1>00s</h1></button>



</div>

<div class="itemsection">
  
  <div class="search">
    <h1 class="green">{{ msg }}</h1>

    <div class="bgImage" :style="{ backgroundImage: 'linear-gradient(rgba(0, 0, 0, 0.5), rgba(0, 0, 0, 0.5)), url(' + backgroundSlideImg + ')' }">
    <div class="itemWrapper">
    

      
        <h1>{{ theData['title'] }}</h1>
        <p class='est'>{{ "EST." + theData['productionDates']?.[0]?.['fromYear'] || 'N/A' }}</p>
        <p>{{ theData['summary'] }}</p>
        <img>

          <button class="itemFetch" @click="fetchData"><img src="./icons/down-arrow.png" alt="buttonpng"/></button>
        </div>
  </div> 
</div>
</div>
<div class="detailedSortWrapper">


  <ul class="itemGrid"> 
  <li class="gridItem" v-for="(item, index) in paginatedItems" :key="index"> 
    <div class="itemContainer" v-for="(url, urlIndex) in item.URL" :key="urlIndex">
      <a :href="`https://www.collection.nfsa.gov.au/title/${item.itemId}`">
        <img :src="url" :alt="`Image ${urlIndex + 1} for Item ${index + 1}`" />
        <h3>{{ listNames[index] }}</h3>
      </a>
    </div>
  </li>
</ul>

<div class="pagination">
  <button @click="currentPage = Math.max(currentPage - 1, 1)" :disabled="currentPage === 1">Previous</button>
  <span>Page {{ currentPage }} of {{ totalPages }}</span>
  <button @click="currentPage = Math.min(currentPage + 1, totalPages)" :disabled="currentPage === totalPages">Next</button>
</div>
   
   <!-- <img v-bind:src="item.URL[index]" :alt="`Item ${index + 1}`" />  -->
<!-- <div id="item1"></div>
  <img src="currentitemset.items[0]?.URL" alt="Item Image" /> -->

  </div>
</template>
<style scoped>

@font-face {
    font-family: DSEGClassicBold;
    src: url(DSEG14Classic-Bold.ttf);
}

.itemSection {

  display: block;
}

.decadeSelectWrapper{

  display: flex;
  padding-bottom: 1rem;
  z-index: 1;
}
.decadeSelectWrapper button{

  height: 100dvh;
  background-color: white;
  border: 3px solid rgb(74, 74, 74);
  color: black;
  padding: 15px 32px;
  text-align: center;
  text-decoration: none;
  display: inline-block;
  font-size: 16px;
  width: 25vw;
  border-radius: 20px;
  z-index: 2;

}

.decadeSelectWrapper button:hover{

  background-color: #ffeb37;
}

.decadeSelectWrapper h1 {
  text-shadow: none;
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
  rotate: 270deg;
}
p{ 
  font-family: "PT Sans", serif;
  font-size: 1.3rem;
  line-height: 1.5;
  width: 37.5em;
  color: white;
}
h1 {
  margin-top: 0.5rem;
  color: #ffeb37;
  text-shadow: 0 0 5px #ffeb37, 0 0 10px #ffeb37;
  font-weight: 500;
  font-size: 2.6rem;
}

.est {

  font-family: DSEGClassicBold;
  font-size: 2.2em;
  color: #ffeb37;
  padding-bottom: 3rem;
  text-shadow: 0 0 5px #ffeb37, 0 0 10px #ffeb37;
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
  top: -3rem;
  z-index: 0;
  border-radius: 20px;
}

.itemGrid {

  list-style-type: none;
  display: grid;
  grid-template-columns: repeat(3, 0.5fr);


}

.itemContainer {

  position:relative;
}

.itemContainer img{
border-radius: 10px;
width: 40%;
height: 40%;
position: relative;
transition: all 0.2s ease-out;
}

.itemContainer:hover img {

  filter: brightness(50%);
}

.itemContainer h3 {

display: none;
font-size: small;
font-weight: bold;
position: absolute;
top: 10%;
left: 10%;
z-index: 3;
color: #ffeb37;
margin: auto;
text-align: center;
max-width: 7em;

}

.itemContainer:hover h3 {

  display: inherit;
  position: absolute;
  cursor: pointer;
  text-shadow: 0 0 5px #ffeb37, 0 0 10px #ffeb37;

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
