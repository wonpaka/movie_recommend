<template>
  <div class="d-flex flex-wrap" v-if="movieList">
    <!-- 카드 섹션 하나 -->
    <div
      class="movie-card"
      style="width:200px;"
      v-for="li in movieList"
      :key="li.id"
    >
      <div  @click="goDetail(li.id)" v-if="li">
        <div class="d-flex justify-content-end movie-date" v-if="li.release_date">{{ li.release_date.split("-")[0] }}</div>
        <b-img class="rounded d-flex justify-content-center" style="width:220px; height:280px;" v-if="li.poster_path" fluid :src="image(li.poster_path)" alt="Image 2"></b-img>
        <b-img class="rounded d-flex justify-content-center" style="width:220px; height:280px;" v-else fluid :src="noImage" alt="Image 2"></b-img>
        <div class="d-flex justify-content-center movie-information"> 
          <div class="movie-title text-truncate" v-if="li.title.length > 12">{{ li.title }}</div>
          <div class="d-flex justify-content-center movie-title" v-else>{{ li.title }}</div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>

export default {
  props: ['movieList'],
  data(){
    return {
      noImage: require("../assets/error.jpg")
    }
  }
,
  methods: {
    image(img) {
      return `https://image.tmdb.org/t/p/w300/${img}`
    },
     goDetail(id){
      // console.log(id)
      this.$router.push(`detail/${id}`)
    }
  },
}
</script>

<style>
.movie-date { 
  color: gray;
  opacity: 0.8;
  font-size: 12px;
}
.movie-title { 
  font-size: 14px;
}
</style>
