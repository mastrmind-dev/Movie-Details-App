<template>
  <div class="home">
    <div class="feature-card">
      <router-link to="/movie/tt0409591">
        <img
          src="https://img.cdn-pictorem.com/uploads/collection/I/IB5PAB9RBI/900_Anime_7_1608090041.5705.jpg"
          alt="naruto-poster"
          class="featured-img"
        />
        <div class="detail">
          <h3>Naruto</h3>
          <p>
            Naruto Uzumaki, a mischievous adolescent ninja, struggles as he
            searches for recognition and dreams of becoming the Hokage, the
            village's leader and strongest ninja.
          </p>
        </div>
      </router-link>
    </div>
    <form @submit.prevent="searchMovies()" class="search-box">
      <input
        type="text"
        placeholder="What are you searching for?"
        v-model="search"
      />
      <input type="submit" value="search" />
    </form>
    <div class="movies-list" v-for="movie in movies" :key="movie.imdbID">
      <router-link :to="'/movie/' + movie.imdbID" class="movie-link">
        <div class="product-image">
          <img :src="movie.Poster" alt="Movie Poster" />
          <div class="type">{{ movie.Type }}</div>
          <div class="detail">
            <p class="y">{{ movie.Year }}</p>
            <h3>{{ movie.Title }}</h3>
          </div>
        </div>
      </router-link>
    </div>
  </div>
</template>

<script>
import { ref } from "vue";
import env from "@/env";
export default {
  setup() {
    const search = ref("");
    const movies = ref([]);

    const searchMovies = () => {
      if (search.value) {
        console.log("search value=>");
        console.log(search.value);
        fetch(`http://www.omdbapi.com/?apikey=${env.apikey}&s=${search.value}`)
          .then((response) => response.json())
          .then((data) => {
            console.log(data.Search);
            movies.value = data.Search;
            search.value = "";
          });
      }
    };

    return { search, movies, searchMovies };
  },
};
</script>

<style lang="scss">
.home {
  .feature-card {
    position: relative;
  }
  .featured-img {
    display: block;
    width: 100%;
    height: 100px;
    object-fit: cover;
    position: relative;
    z-index: 0;
  }
  .detail {
    position: absolut;
    left: 0;
    right: 0;
    bottom: 0;
    background-color: rgba(0, 0, 0, 0.6);
    padding: 16px;
    z-index: 1;
  }
  h3 {
    color: #fff;
    margin-bottom: 16px;
  }

  p {
    color: #fff;
  }

  .search-box {
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
    padding: 16px;

    input {
      display: block;
      appearance: none;
      border: none;
      outline: none;
      background: none;

      &[type="text"] {
        width: 100%;
        color: #fff;
        background-color: #496583;
        font-size: 20px;
        padding: 10px 16px;
        border-radius: 8px;
        margin-bottom: 15px;
        transition: 0.4s;
      }

      &::placeholder {
        color: #f3f3f3;
      }

      &:focus {
        box-shadow: 0px 3px 6px rgba(0, 0, 0, 0.2);
      }
      &[type="submit"] {
        width: 100%;
        max-width: 300px;
        background-color: #42b883;
        padding: 16px;
        border-radius: 8px;
        color: #fff;
        font-size: 20px;
        text-transform: uppercase;
        transition: 0.4s;

        &:active {
          background-color: #3b8070;
        }
      }
    }
  }

  .movies-list {
    display: flex;
    flex-wrap: wrap;
    margin: 0 8px;

    .movie {
      max-width: 50%;
      flex: 1 1 50%;
      padding: 16px 8px;

      .movie-link {
        display: flex;
        flex-direction: column;
        height: 100%;

        .product-image {
          position: relative;
          display: block;
          img {
            display: block;
            width: 100%;
            height: 275px;
            object-fit: cover;
          }
          .type{
            position:absolute;
            padding:8px 16px;
            background-color: #42b883;
            color:#fff;
            bottom:16px;
            left:0px;
            text-transform: capitalize;
          }
          .detail{
            background-color: #496583;
            padding: 16px 8px;
            flex: 1 1 100%;
            border-radius: 0 0 8px 8px;
            .y{
              color: #AAA;
              font-size: 14px;
            }
            h3{
              color: #fff;
              font-weight: 600;
              font-size: 18px;
            }
          }

        }
      }
    }
  }
}
</style>
