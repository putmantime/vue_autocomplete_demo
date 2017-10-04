<template>
  <div class="hello container-fluid">
    <div class="row">
      <div class="col-md-4"></div>
      <div class="col-md-4">
        <h1> Bacterial Genus </h1>
        <autocomplete :suggestions="suggestions" v-model="selection"></autocomplete>
        <br/>
        {{ selection }}
      </div>
      <div class="col-md-4"></div>
    </div>
  </div>
</template>

<script>
  import axios from 'axios';
  import autocomplete from './Autocomplete';

  export default {
    name: 'hello',
    components: {
      autocomplete,
    },
    data() {
      return {
        selection: '',
        suggestions: [],
      };
    },
    methods: {
      fetchData() {
        const endpoint = 'https://query.wikidata.org/sparql?format=json&query=';
        const query = 'SELECT DISTINCT ?taxon ?taxonLabel ?taxid ' +
          'WHERE { ' +
          '?taxon wdt:P685 ?taxid; ' +
          'wdt:P171* wd:Q10876; ' +
          'wdt:P105 wd:Q34740; ' +
          'rdfs:label ?taxonLabel. ' +
          'FILTER (LANG(?taxonLabel) = "en")}';

        axios.get(endpoint + query)
          .then((resp) => {
          // eslint-disable-next-line
            console.log(resp.data.results.bindings);
            this.suggestions = resp.data.results.bindings;
          })
          .catch((err) => {
          // eslint-disable-next-line
              console.log(err)
        // eslint-disable-next-line
          });
        // eslint-disable-next-line
        },
    },
    mounted() {
      this.fetchData();
    },
  };
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
  h1, h2 {
    font-weight: normal;
  }

  ul {
    list-style-type: none;
    padding: 0;
  }

  li {
    display: inline-block;
    margin: 0 10px;
  }

  a {
    color: #42b983;
  }
</style>


// eslint-disable-next-line

