<template>
  <div>
    <div class="container d-flex flex-wrap" v-if="nowPlaying">
      <div class="h4 ml-3 mt-5 mb-4">
        <b-icon icon="film" style="width: 25px; height: 25px;"></b-icon> 
       현재상영작  
        <b-icon icon="film" style="width: 25px; height: 25px;"></b-icon> 
      </div>
      <MovieLists :movieList="nowPlaying"></MovieLists>
      <div class="h4 ml-3 mt-5 mb-4">
        <b-icon icon="film" style="width: 25px; height: 25px;"></b-icon> 
       인기상영작  
        <b-icon icon="film" style="width: 25px; height: 25px;"></b-icon> 
      </div>
      <MovieLists :movieList="popular"></MovieLists>
      <div class="h4 ml-3 mt-5 mb-4">
        <b-icon icon="film" style="width: 25px; height: 25px;"></b-icon> 
       개봉예정작  
        <b-icon icon="film" style="width: 25px; height: 25px;"></b-icon> 
      </div>
      <MovieLists :movieList="upComming"></MovieLists>
    </div>
  </div>
</template>

<script>
import MovieLists from "../components/MovieLists"
import MovieText from "../components/MovieText"
import { movieApi } from "../utils/axios"
import { mapMutations } from "vuex"
export default {
  data() {
    return {
      nowPlaying: {},
      popular: {},
      upComming: {},
    };
  },
  components: {

    MovieText,
    MovieLists,
  },
  methods: {
    ...mapMutations(["SET_LOADING"]),

  },
  
  created() {
    this.SET_LOADING(true)
  },
  async mounted() {
    try {
      // vuex를 통해서 로딩을 없애준다.
      const { data } = await movieApi.nowPlaying()
      console.log(data.results)
      this.movieList = data.results
      const { nowPlaying, popular, upComing } = movieApi
      const requestArr = [nowPlaying, popular, upComing]
      const [now, pop, up] = await Promise.all(
        requestArr.map((li) => li().then((res) => res.data.results))
      )
      console.log("pop")
      console.log(pop)
      this.SET_LOADING(false)
      this.nowPlaying = now
      this.popular = pop
      this.upComming = up
    } catch (error) {
      this.movieList = "해당 자료가 존재하지 않습니다."
    }
  },
}
</script>

<style>
.movie-card {
  margin: 12px;
  width: 200;
  font-size: 20px;
  font-weight: 400;
}
.movie-card:hover {
  opacity: 0.5;
  cursor: pointer;
}
.movie-card > img {
  height: 180px;
  border-radius: 8px;
}
.movie-information {
  margin-top: 7px;
}
</style>
