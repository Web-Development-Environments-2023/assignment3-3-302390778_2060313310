<template>
  <div class="container">
    <br><br>
    <b-row>
      <b-col md="8">
        <div id="mainSearch">
          <b-row>
            <b-col md="8">
              <b-form-input id="q" size="lg" v-model="query" placeholder="What are you loocking for?"></b-form-input>
            </b-col>
            <b-col md="4">
              <b-button style="align-content: center;" @click="search" :disabled="query===''" class="btn" pill variant="outline-primary">Search</b-button>
            </b-col>
          </b-row>
        </div>
      </b-col>
      <b-col md="4">
        <div id="filter">
          <div id="search-amount">
            <div class="mt-3">amount: <strong>{{ selectedAmount }}</strong></div>
            <b-form-select v-model="selectedAmount" class="mb-3">
              <b-form-select-option value="5">5</b-form-select-option>
              <b-form-select-option value="10">10</b-form-select-option>
              <b-form-select-option value="15">15</b-form-select-option>
            </b-form-select>
          </div>
          <div id="search-cuisine">
            <div class="mt-3">cuisine: <strong>{{ selectedCuisine }}</strong></div>
            <b-form-select v-model="selectedCuisine" class="mb-3">
              <b-form-select-option value="African">African</b-form-select-option>
              <b-form-select-option value="American">American</b-form-select-option>
              <b-form-select-option value="British">British</b-form-select-option>
              <b-form-select-option value="Cajun">Cajun</b-form-select-option>
              <b-form-select-option value="Caribbean">Caribbean</b-form-select-option>
              <b-form-select-option value="Chinese">Chinese</b-form-select-option>
              <b-form-select-option value="Eastern European">Eastern European</b-form-select-option>
              <b-form-select-option value="European">European</b-form-select-option>
              <b-form-select-option value="German">German</b-form-select-option>
              <b-form-select-option value="Greek">Greek</b-form-select-option>
              <b-form-select-option value="Indian">Indian</b-form-select-option>
              <b-form-select-option value="Irish">Irish</b-form-select-option>
              <b-form-select-option value="Italian">Italian</b-form-select-option>
              <b-form-select-option value="Japanese">Japanese</b-form-select-option>
              <b-form-select-option value="Jewish">Jewish</b-form-select-option>
              <b-form-select-option value="Korean">Korean</b-form-select-option>
              <b-form-select-option value="Latin American">Latin American</b-form-select-option>
              <b-form-select-option value="Mediterranean">Mediterranean</b-form-select-option>
              <b-form-select-option value="Mexican">Mexican</b-form-select-option>
              <b-form-select-option value="Middle Eastern">Middle Eastern</b-form-select-option>
              <b-form-select-option value="Nordic">Nordic</b-form-select-option>
              <b-form-select-option value="Southern">Southern</b-form-select-option>
              <b-form-select-option value="Spanish">Spanish</b-form-select-option>
              <b-form-select-option value="Thai">Thai</b-form-select-option>
              <b-form-select-option value="Vietnamese">Vietnamese</b-form-select-option>
            </b-form-select>
          </div>
          <div id="search-diet">
            <div class="mt-3">diet: <strong>{{ selectedDiet }}</strong></div>
            <b-form-select v-model="selectedDiet" class="mb-3">
              <b-form-select-option value="Vegetarian">Vegetarian</b-form-select-option>
              <b-form-select-option value="Gluten Free">Gluten Free</b-form-select-option>
              <b-form-select-option value="Vegan">Vegan</b-form-select-option>
            </b-form-select>
          </div>
          <div id="search-intolerance">
            <div class="mt-3">intolerance: <strong>{{ selectedIntolerance }}</strong></div>
            <b-form-select v-model="selectedIntolerance" class="mb-3">
              <b-form-select-option value="Peanut">Peanut</b-form-select-option>
              <b-form-select-option value="Dairy">Dairy</b-form-select-option>
              <b-form-select-option value="Egg">Egg</b-form-select-option>
              <b-form-select-option value="Gluten">Gluten</b-form-select-option>
              <b-form-select-option value="Grain">Grain</b-form-select-option>
              <b-form-select-option value="Seafood">Seafood</b-form-select-option>
              <b-form-select-option value="Sesame">Sesame</b-form-select-option>
              <b-form-select-option value="Shellfish">Shellfish</b-form-select-option>
              <b-form-select-option value="Soy">Soy</b-form-select-option>
              <b-form-select-option value="Sulfite">Sulfite</b-form-select-option>
              <b-form-select-option value="Tree Nut">Tree Nut</b-form-select-option>
              <b-form-select-option value="Wheat">Wheat</b-form-select-option>
            </b-form-select>
          </div>
        </div>
      </b-col>
    </b-row>
    <b-row id="sortRow">
          <div id="sort">
            <b-form-select v-model="sortBy" class="mb-3">
              <b-form-select-option value="likes">Popularity</b-form-select-option>
              <b-form-select-option value="time">Ready in minutes</b-form-select-option>
            </b-form-select>
            <b-button :disabled="sortBy==='' || !searchedBefore"  id="btn"  pill variant="light" @click="sortRes">Sort</b-button> 
          </div>
    </b-row>
    <b-row>
      <div v-if="show">
        <h2 v-show="isNoRes" class="headertekst">
          No match results
        </h2>
        <h2 v-show="!isNoRes" class="headertekst">
          Recipes that found:
        </h2>
      </div>
      <h2 v-if="!show && $root.store.username" class="headertekst">Last search</h2>
      <RecipePreviewList id="result" v-if="show" v-on:recipes_length="recipes_len"
        :key="searchKey"
        random="search"
        v-bind:query="query"
        v-bind:searchCuisine="selectedCuisine"
        v-bind:searchIntol="selectedIntolerance"
        v-bind:searchDiet="selectedDiet"
        v-bind:searchSort="selectedSort"
        v-bind:searchAmount="selectedAmount"
        ref="rpl">
      </RecipePreviewList>

      <RecipePreviewList id="result" v-else
        random="lastSearch"
        ref="rpl2">
      </RecipePreviewList>
    </b-row>
  </div>
</template>

<script>
import RecipePreviewList from "../components/RecipePreviewList";
  export default{
    name: "searchPage",
    components: {
        RecipePreviewList
    },
    data(){
      return{
        // resTitle: "Recipes that found",
        query:"",
        show:false,
        searchKey:0,
        selectedSort: null,
        selectedIntolerance: '',
        selectedDiet: '',
        selectedCuisine: '',
        selectedAmount: "5",
        sortBy: "",
        searchedBefore: false,
        isNoRes: true
        };
    },
    created(){
      this.show=false;
      this.query="";
      this.searchKey = 0;
      this.noRes = true;
    },
    methods: {
    search() {
      if (this.query != ""){
        this.show=true;
        this.searchKey++;
        this.searchedBefore = true;
      }
    },
    sortRes(){
      if (this.sortBy != "" ){
        this.$refs.rpl.sort(this.sortBy)
      }
    },
    recipes_len(len){
      this.isNoRes = (len == 0)
    }
    }
  }
</script>

<style>
/* #q, #queryCol{
  width: 450px;
} */
#mainSearch{
  align-self: center;
  align-content: center;
  align-items: center;
}
#result{
    /* align-self: center; */
    align-content: center;
    /* align-items: center; */
    /* padding-top: 100px; */
}
#filter{
  width: 40%;
  float: left;
}
#sort{
  width: 25%;
  float: right;
}

#btn{
  width: 1.6cm;
  float: right;
}
.headertekst{
  text-align: center;
}

</style>

