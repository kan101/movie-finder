<template>
  <div class="container-fluid">
    <h1 class="title">Movie Finder</h1>
    <form class="search-form mb-4">
      <div class="form-row align-items-center">
        <div class="col">
          <label class="sr-only" for="inlineFormInputGroup">Search</label>
          <div class="input-group mb-2">
            <input
              type="text"
              class="form-control"
              id="inlineFormInputGroup"
              placeholder="Search by Title"
              v-model="searchTerm"
              @input="search"
            />
            <div class="input-group-append pointer" @click="search">
              <div class="input-group-text">Search</div>
            </div>
          </div>
        </div>
      </div>
    </form>
    <div v-show="movies.length > 0">
      <h3 class="search-results text-center mb-3">
        {{ numberWithCommas(rows) }} results
      </h3>
      <!-- <carousel>
        <slide v-for="(movie, m) in movies" :key="m" :perPage="1">
          <div class="card mr-3">
            <img :src="movie.Poster" class="card-img-top" alt="Movie Poster" />
            <div class="card-body">
              <h5 class="card-title">{{movie.Title}}</h5>
              <p class="card-text">
                {{movie.Year}}
              </p>
              <a href="#" class="btn btn-primary">Go somewhere</a>
            </div>
          </div>
        </slide>
      </carousel> -->
      <div class="wrapper">
        <b-carousel
          id=""
          :interval="0"
          controls
          indicators
          img-width="220"
          img-height="480"
        >
          <article
            v-for="(movie, m) in movies"
            :key="m"
            class="pointer"
            @click="showDetails(movie.imdbID)"
          >
            <b-carousel-slide
              :img-src="
                movie.Poster === 'N/A' ? '/img/images/reel.png' : movie.Poster
              "
              :caption="movie.Title"
              :text="movie.Year"
            >
            </b-carousel-slide>
          </article>
        </b-carousel>
        <b-pagination
          v-model="currentPage"
          :total-rows="rows"
          :per-page="perPage"
          @input="search"
          align="center"
        ></b-pagination>
      </div>
    </div>
    <div v-show="loading">
      <div
        class="loader position-fixed d-flex align-items-center justify-content-center"
      >
        <b-spinner variant="success" type="grow" label="Spinning"></b-spinner>
      </div>
    </div>
    <DetailsModal :info="details"></DetailsModal>
  </div>
</template>

<script>
// @ is an alias to /src

import axios from "../axios.config";
import _ from "lodash";

import DetailsModal from "@/components/modals/DetailsModal.vue";

export default {
  name: "Home",
  components: {
    DetailsModal,
  },
  data() {
    return {
      movies: [],
      apiKey: process.env.VUE_APP_API_KEY,
      searchTerm: "",
      loading: false,
      rows: 0,
      currentPage: 1,
      perPage: 10,
      details: {},
    };
  },
  methods: {
    showDetails(id) {
      this.loading = true;
      let url = `?i=${id}&apikey=${this.apiKey}`;

      axios
        .post(url)
        .then((response) => {
          this.details = response.data;
          this.$bvModal.show("detailsModal");
          this.loading = false;
        })
        .catch((error) => {
          console.log(error);
          this.$bvToast.toast(error, {
            variant: "danger",
            solid: true,
          });
          this.loading = false;
        });
    },

    search: _.debounce(function() {
      this.loading = true;

      let url = `?s=${this.searchTerm}&apikey=${this.apiKey}&page=${this.currentPage}`;
      axios
        .post(url)
        .then((response) => {
          this.movies = response.data.Search;
          this.rows = response.data.totalResults;
          this.perPage = 10;
          this.loading = false;
        })
        .catch((error) => {
          console.log(error);
          this.$bvToast.toast(error, {
            variant: "danger",
            solid: true,
          });
          this.loading = false;
        });
    }, 1000),

    numberWithCommas(x) {
      x = parseFloat(x)
        .toFixed(2)
        .replace(/\.00$/, "");
      var parts = x.toString().split(".");
      parts[0] = parts[0].replace(",", "");
      parts[0] = parts[0].replace(/\B(?=(\d{3})+(?!\d))/g, ",");
      return parts.join(".");
    },
  },

  mounted() {},
};
</script>
<style lang="scss">
.title {
  text-align: center;
  padding: 20px;
  font-size: 33px;
}

.search-form {
  width: 50%;
  margin: auto;
  min-width: 250px;
  max-width: 700px;

  input {
    height: 62px;
    font-size: 18px;
  }
}

.search-results {
  font-size: 22px;
  font-weight: normal;
  color: #655;
}

.pointer {
  cursor: pointer;
}

.loader {
  width: 100%;
  height: 100%;
  z-index: 999999;
  top: 0;
  left: 0;
}

.wrapper {
  width: 70%;
  max-width: 350px;
  min-width: 250px;
  margin: auto;

  .carousel-inner {
    border-radius: 3px;

    img {
      height: 430px;
    }

    .carousel-caption {
      position: static !important;
      color: #333;
      background: #eee;
      padding: 8px 8px 3px;

      h3 {
        font-size: 21px;
        text-overflow: ellipsis;
        overflow: hidden;
        white-space: nowrap;
      }

      p {
        margin-bottom: 0;
      }
    }
  }

  .carousel-indicators {
    position: static;

    li {
      background-color: #000;
    }
  }
}
</style>
