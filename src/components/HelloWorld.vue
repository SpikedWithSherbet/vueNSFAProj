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

<div class="app-background">

<section class="hero">

<a><h1>Explore a <i>detailed</i> history</h1></a>

<span><h3>DECADES <br>
  &darr;</h3></span>

</section>

<div class="decadeSelectWrapper">

<a href='#itemShowcase'><button class="70sWrapper" @click="changeto70s"><h1>70s</h1></button></a>
<a href='#itemShowcase'><button class="80sWrapper" @click="changeto80s"><h1>80s</h1></button></a>
<a href='#itemShowcase'><button class="90sWrapper" @click="changeto90s"><h1>90s</h1></button></a>
<a href='#itemShowcase'><button class="00sWrapper" @click="changeto00s"><h1>00s</h1></button></a>



</div>


<div class="itemsection">
  
  <div class="search">
    <h1 class="green">{{ msg }}</h1>

    <div class="bgImage" :style="{ backgroundImage: 'linear-gradient(rgba(0, 0, 0, 0.5), rgba(0, 0, 0, 0.5)), url(' + backgroundSlideImg + ')' }">
      <div class="itemWrapper" id="itemShowcase">
    
        
      
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

  <h1>EXTRA INFO</h1>

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
  <button class="Previous" @click="currentPage = Math.max(currentPage - 1, 1)" :disabled="currentPage === 1"><img src="./icons/down-arrow.png" alt="buttonpng" style="rotate: 90deg;"/></button>
  <span><h2>{{ currentPage }}/{{ totalPages }}</h2></span>
  <button class="Next" @click="currentPage = Math.min(currentPage + 1, totalPages)" :disabled="currentPage === totalPages"><img src="./icons/down-arrow.png" alt="buttonpng" style="rotate: 270deg;"/></button>
</div>
   
   <!-- <img v-bind:src="item.URL[index]" :alt="`Item ${index + 1}`" />  -->
<!-- <div id="item1"></div>
  <img src="currentitemset.items[0]?.URL" alt="Item Image" /> -->

  </div>
</div>
</template>
<style scoped>

a {

  text-decoration: none;
}
@font-face {
    font-family: DSEGClassicBold;
    src: url(DSEG14Classic-Bold.ttf);
}

.app-background {

  background-color: #333;
}

.hero {

  height: 100%;
  background-image: linear-gradient(to top, #000000, #000000, #333333, #333333);
  background-size: cover;
  background-size: 100% 1px;

}

.hero h1 {

  color: white;
  font-family: 'PT sans';
  font-size: 8rem;
  width: 65%;
  margin-left: 2rem;
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
  padding: 15px 32px;
  text-align: center;
  text-decoration: none;
  display: inline-block;
  font-size: 16px;
  width: 25vw;
  border-radius: 20px;
  z-index: 2;
  transition: all ease-in ;

}

.decadeSelectWrapper button:hover{

  background-color: #ffeb37;
  cursor: pointer;
}

.decadeSelectWrapper h1 {
  text-shadow: none;
  color: black;
  font-family: 'PT Sans', sans-serif;
  font-weight: 700;
}

.crteffect {
 
  background-size: cover;
  background-size: 100% 1px;
  z-index: 3;
  position:relative;
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
  font-family: "PT Sans", sans-serif;
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

.detailedSortWrapper {

  display: grid;
  justify-content: center;
  height: 100%;
  align-items: center;
  justify-items: center;
}

.itemGrid {
  list-style-type: none;
  display: grid;
  grid-template-columns: repeat(3, 1fr); /* Adjust this for the desired number of columns */
  justify-content: center;
  grid-gap: 10px;
}

.gridItem {
  display: flex; /* Use flexbox for alignment */
  flex-direction: column; /* Stack children vertically */
  align-items: center; /* Center align */
  justify-content: center; /* Center align */
  width: 100%; /* Ensures it takes full width of the grid cell */
  height: auto; /* Adjusts height automatically based on content */
}

.itemContainer {
  box-sizing: border-box; /* Adjusts box model for consistent sizing */
  position: relative;
  width: 20em; /* Takes full width of the grid cell */
  height: 25em; /* Takes full height of the grid cell */
}

.itemContainer img {
  border-radius: 10px;
  width: 20em; /* Ensures image fills container */
  height: 20em; /* Maintains aspect ratio */
  object-fit: cover; /* Keeps aspect ratio and covers the entire area */
}

.itemContainer h3 {
  display: none;
  font-size: large;
  font-weight: bold;
  position: absolute;
  bottom: 50%;
  left: 30%;
  z-index: 3;
  color: #ffeb37;
  margin: auto;
  text-align: center;
  max-width: 7em;
}

.itemContainer:hover img {

  filter: brightness(50%);
}

.itemContainer:hover h3 {

  display: inherit;
  position: absolute;
  cursor: pointer;
  text-shadow: 0 0 5px #ffeb37, 0 0 10px #ffeb37;

}

.pagination {

padding-top: 0em;
padding-bottom: 2em;
display: flex;
align-items: center;
gap: 0.3rem;

}

span h2 {

font-weight: bold;
font-family: 'DSEGCLassicBold';
color: white;

}

span h3 {

text-align: center;

padding: 2rem;
color: white;
font-size: 1.5rem;
font-weight: bold;
}

.greetings h1,
.greetings h3 {
  text-align: center;
}

@media screen{
  .hero {
    animation: scanlines infinite 55s linear ;
  }
} 
@keyframes scanlines {
  from {
    background-position: 0 0;
  }
  to {
    background-position: 0 -10px;
  }
}

@media (min-width: 1024px) {
  .greetings h1,
  .greetings h3 {
    text-align: left;
  }
}

@media (max-width: 768px) {

  .itemGrid {
  list-style-type: none;
  display: grid;
  grid-template-columns: repeat(2, 1fr); }

  .hero h1 {
font-size: 6rem;
width: 65%;
margin-left: 2rem;
}
  
}

@media (max-width: 426px) {

.itemGrid {
list-style-type: none;
display: grid;
grid-template-columns: repeat(1, 1fr); }

.decadeSelectWrapper{

display: block;
}
.decadeSelectWrapper button{

height: 25vh;
background-color: white;
border: 3px solid rgb(74, 74, 74);
padding: 15px 32px;
text-align: center;
text-decoration: none;
display: block;
font-size: 16px;
width: 100vw;
border-radius: 20px;
z-index: 2;
transition: all ease-in ;

}

.hero h1 {
font-size: 3.7rem;
width: 70%;
margin-left: 2rem;
}

.itemGrid {

  padding: 0;
}

}
</style>
