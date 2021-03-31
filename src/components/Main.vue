<template>
  <div class="container">
    <div class="header">
      <form @submit="submitQuery">
        <label for="query">Search</label>
        <input id="query" v-model="query" type="text" name="query" />
        <input type="submit" value="Submit" />
      </form>
      <div class="sort">
        <div>
          Sort Alphabetically
          <button
            v-on:click="sortOrder = 1"
            v-if="sortOrder == -1 || sortOrder == 0"
          >
            ↓
          </button>
          <button v-on:click="sortOrder = -1" v-if="sortOrder == 1">
            ↑
          </button>
          <button v-on:click="sortOrder = 0">(remove sort)</button>
        </div>
      </div>
    </div>
    <ul v-if="articles && articles.length">
      <li v-for="(item, index) in articles" :key="index">
        <div class="col image">
          <img
            v-bind:src="item.urlToImage"
            v-on:click="modalImage = item.urlToImage"
          />
        </div>
        <div class="col">
          <a v-bind:href="item.url">{{ item.title }}</a>
          <div class="byline">
            {{ item.source.name
            }}{{ item.source && item.source.name && item.author && " - "
            }}{{ item.author }}
          </div>
          <div class="desc">{{ item.description }}</div>
        </div>
      </li>
    </ul>
    <div v-if="!articles || !articles.length">
      <div v-if="searchTerm && searchTerm !== ''">
        No results found for "{{ searchTerm }}"
      </div>
      <div v-if="!searchTerm || searchTerm === ''">
        Please enter a search term
      </div>
    </div>
  </div>
  <Modal v-bind:image="modalImage" v-on:close="modalImage = null" />
</template>

<script>
// was using this earlier because the free api endpoint has a limit of 100 calls
// import testData from "../testData.js";
import Modal from "./Modal.vue";

export default {
  name: "Main",
  props: {
    msg: String,
  },
  components: {
    Modal,
  },
  data() {
    return {
      info: null,
      query: "",
      searchTerm: "",
      sortOrder: 0,
      articles: [],
      modalImage: null,
      data: [],
    };
  },
  methods: {
    processData: function(data) {
      this.data = data.articles;
      this.sortData();
    },
    sortData: function() {
      let newDataArr = [...this.data];

      if (this.sortOrder !== 0) {
        this.articles = newDataArr.sort((a, b) => {
          let returnNum = 0;
          const titleA = a.title.toUpperCase();
          const titleB = b.title.toUpperCase();
          if (titleA < titleB) {
            returnNum = -1;
          }
          if (titleA > titleB) {
            returnNum = 1;
          }
          return returnNum * this.sortOrder;
        });
      } else {
        this.articles = this.data;
      }
    },
    queryTerm: function(term) {
      // console.log(term);
      // this.processData(testData);

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
    },
    submitQuery: function(e) {
      this.searchTerm = this.query;
      this.queryTerm(this.searchTerm);
      e.preventDefault();
    },
    showImage: function() {},
  },
  watch: {
    sortOrder: function() {
      this.sortData();
    },
  },
  mounted() {},
};
</script>

<style lang="scss" scoped>
.container {
  font-size: 16px;
  padding: 1em;
}
a {
  font-weight: 700;
  color: inherit;
}
ul {
  display: block;
  text-align: left;
  list-style: none;
  margin: 0;
  padding: 0;

  li {
    margin-bottom: 2em;
    display: flex;
    flex-direction: column;
    .col {
      flex: 4;
      &.image {
        flex: 1;
        min-width: 180px;
        margin-bottom: 1em;
      }
    }
  }
}

.header {
  position: sticky;
  top: 0;
  background: #fff;
  padding: 1em 0;
  form {
    margin-bottom: 10px;
  }
  button {
    appearance: none;
    border: none;
    background: none;
    cursor: pointer;
  }
}
.byline {
  font-size: 0.8em;
  color: #666;
  margin: 0.8em 0;
}
.desc {
  display: -webkit-box;
  -webkit-line-clamp: 3;
  -webkit-box-orient: vertical;
  overflow: hidden;
}
.image {
  img {
    max-width: 100%;
    cursor: pointer;
  }
}
input {
  margin-left: 10px;
}
@media (min-width: 769px) and (max-width: 922px) {
}
@media (min-width: 481px) and (max-width: 768px) {
}

@media (min-width: 480px) {
  ul {
    li {
      flex-direction: row;
      .col {
        &.image {
          margin-right: 1em;
        }
      }
    }
  }
}
</style>
