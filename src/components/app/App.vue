<template>
  <div class="app font-monospace">
    <div class="content p-5">
      <AppInfo
        :moviesCount="movies.length"
        :favouriteMoviesCount="movies.filter((c) => c.favourite).length"
      />
      <Box>
        <SearchPanel :updateTermHandler="updateTermHandler" />
        <AppFilter @updateFilter="updateFilter" />
      </Box>
      <Box v-if="!movies.length">
        <h2>Kinolar yo'q</h2>
      </Box>
      <MovieList
        v-else
        :movies="onFilterHandler(searchFunc(movies, term), filter)"
        @onToggle="onToggleHandler"
      />
      <MovieAddForm @createMovie="createMovie" />
    </div>
  </div>
</template>

<script>
import AppInfo from "../app-info/AppInfo.vue";
import AppFilter from "../app-filter/AppFilter.vue";
import MovieAddForm from "../movie-add-form/MovieAddForm.vue";
import MovieList from "../movie-list/MovieList.vue";
import SearchPanel from "../search-panel/SearchPanel.vue";
import axios from "axios";

export default {
  data() {
    return {
      movies: [],
      term: "",
      filter: "all",
    };
  },
  components: {
    AppInfo,
    SearchPanel,
    AppFilter,
    MovieList,
    MovieAddForm,
  },
  methods: {
    createMovie(item) {
      this.movies.push(item);
    },

    onToggleHandler({ id, prop }) {
      this.movies.forEach((movie) => {
        //  My Code
        if (movie.id == id && prop != "remove") {
          movie[prop] = !movie[prop];
        } else if (prop == "remove") {
          this.movies = this.movies.filter((c) => c.id != id);
        }
      });

      // this.movies = this.movies.map(movie => {
      //   if (movie.id == id) {
      //     return {...movie, [prop]: !movie[prop]}
      //   }
      //   return movie
      // })

      // if (prop === 'like') {                        // My Code
      //   this.movies.forEach(movie => {
      //     if (movie.id == obj.id) {
      //       movie.like = !movie.like
      //     }
      //   })
      // } else {
      //   this.movies.forEach(movie => {
      //     if (movie.id == obj.id) {
      //       movie.favourite = !movie.favourite
      //     }
      //   })
      // }
    },
    searchFunc(arr, term) {
      if (term.length == 0) {
        return arr;
      } else {
        return arr.filter((c) => c.name.toLowerCase().indexOf(term) > -1);
      }
    },
    updateTermHandler(term) {
      this.term = term;
    },
    onFilterHandler(arr, filter) {
      switch (filter) {
        case "popular":
          return arr.filter((c) => c.like);
        case "mostViewers":
          return arr.filter((c) => c.viewers > 500);
        default:
          return arr;
      }
    },
    updateFilter(filter) {
      this.filter = filter;
    },

    async fetchmovie() {
      try {
        const { data } = await axios.get(
          "https://jsonplaceholder.typicode.com/posts?_limit=10"
        );

        const newArr = data.map((item) => ({
          id: item.id,
          name: item.title,
          viewers: item.id * Math.floor(Math.random() * 155),
          like: false,
          favourite: false,
        }));

        this.movies = newArr;
      } catch (error) {
        console.log(error);
      }
    },
  },

  mounted() {
    this.fetchmovie();
  },
};
</script>

<style scoped>
.app {
  height: 100vh;
  color: #000;
}

.content {
  width: 1000px;
  min-height: 700px;
  background: #ffffff39;
  margin: 0 auto;
  padding: 5rem 0;
}
</style>
