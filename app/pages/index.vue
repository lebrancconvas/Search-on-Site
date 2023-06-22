<template>
  <div>
    <header>
      <div>
        <h1>Search on Site</h1>
      </div>
    </header>
    <section id="search-section">
      <label for="search-input">Base Website</label>
      <v-text-field id="search-input" name="Website" v-model="website" />
      <div id="search-term" v-for="(term, index) in searchTerms" :key="index">
        <label for="search-input">Search Term</label>
        <v-text-field id="search-input" name="Search Term" v-model="term.searchKeyword" />
        <label for="check-strict">Check Strict</label>
        <v-checkbox id="check-strict" name="Strict" v-model="term.isStrict" />
        <v-btn color="blue" @click="deleteSearchTerm">Delete Search Term</v-btn>
      </div>
    </section>
    <section id="action-section">
      <v-btn color="orange" @click="addSearchTerm">Add Search Term</v-btn>
      <v-btn color="green" @click="searchHandle">Search</v-btn>
      <v-btn color="red" @click="reset">Reset</v-btn>
    </section>
  </div>
</template>

<script lang="ts">
  export default {
    data() {
      return {
        website: '',
        searchTerms: [
          {
            searchKeyword: '',
            isStrict: false
          }
        ],
        search: ''
      }
    },
    methods: {
      addSearchTerm() {
        this.searchTerms.push({
          searchKeyword: '',
          isStrict: false
        })
      },
      searchHandle() {
        // https://www.google.com/search?q=site%3Ayoutube.com+%22Video%22+%2B+Edit+
        let searchTermArray = this.searchTerms.map((term) => {
          if(term.isStrict) {
            term.searchKeyword = `%22${term.searchKeyword}%22`;
          }
          return term.searchKeyword;
        });
        let searchTermJoin = searchTermArray.join('+%2B+');
        this.search = `https://www.google.com/search?q=site%3A${this.website} ${searchTermJoin}`;
        // Redirect to Google Search new tab
        window.open(this.search, '_blank');
      },
      reset() {
        this.website = '';
        this.searchTerms = [
          {
            searchKeyword: '',
            isStrict: false
          }
        ];
        this.search = '';
      },
      deleteSearchTerm() {
        this.searchTerms.pop();
      }
    }
  }
</script>

<style lang="css" scoped>
  header {
    text-align: center;
  }

  #search-section {
    width: 50%;
    margin: 10px auto;
  }

  #search-term {
    margin: 10px 0;
  }

  #action-section {
    margin: 10px auto;
    text-align: center;
  }
</style>
