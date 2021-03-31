<template>
  <form @submit="submitQuery" action="https://vuejs.org/" method="post">
    <label for="query">Search Term</label>
    <input id="query" v-model="query" type="text" name="query" />
    <input type="submit" value="Submit" />
  </form>
</template>

<script>
import testData from "../testData.js";

/* eslint-disable no-debugger */

export default {
  name: "Main",
  props: {
    msg: String,
  },
  data() {
    return {
      info: null,
      query: "tesla",
      data: [],
    };
  },
  methods: {
    processData: function(data) {
      this.data = data.articles;
      console.log(this.data[0]);
    },
    queryTerm: function(term) {
      console.log(term);
      this.processData(testData);

      /*
      fetch(
        `https://newsapi.org/v2/everything?pageSize=20&q=${encodeURI(
          term
        )}&apiKey=3db209faf53643f79427e8b2d2345f6b`
      )
        .then((response) => response.json())
        .then((data) => {
          this.processData(data);
        })
        .catch((error) => {
          console.log(error);
        });
        */
    },
    submitQuery: function(e) {
      this.queryTerm(this.query);
      e.preventDefault();
    },
  },
  mounted() {},
};
/* eslint-enable no-debugger */
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h3 {
  margin: 40px 0 0;
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
