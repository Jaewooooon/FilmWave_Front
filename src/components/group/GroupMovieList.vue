<template>
  <div>
    <h2 class="category-name">그룹 추천 영화</h2>
    <div class="container">
      <button class="scroll-btn btn-left" @click="scrollLeft"><i class="fa-solid fa-arrow-left"></i></button>
      <div class="list-div" ref="slideContainer">
        <div class="slide-div">
          <MovieCard v-for="movie in groupstore.groupMovieList" :key="movie.movie_id" :movie="movie" class="movie-card" />
        </div>
      </div>
      <button class="scroll-btn btn-right" @click="scrollRight"><i class="fa-solid fa-arrow-right"></i></button>
    </div>
  </div>
</template>

<script setup>
import { ref, onMounted } from 'vue'
import { useRoute } from 'vue-router'
import { useGroupStore } from '@/stores/group'
import MovieCard from '@/components/MovieCard.vue'

const groupstore = useGroupStore()
const slideContainer = ref(null)
const route = useRoute()

const groupId = ref(route.params.groupId)

onMounted(() => {
  groupstore.fetchGroupMovie(groupId.value)
})

const scrollLeft = () => {
  if (slideContainer.value) {
    slideContainer.value.scrollBy({
      left: -window.innerWidth * 0.99,
      behavior: 'smooth'
    })
  }
}

const scrollRight = () => {
  if (slideContainer.value) {
    slideContainer.value.scrollBy({
      left: window.innerWidth * 0.99,
      behavior: 'smooth'
    })
  }
}
</script>

<style scoped>
.container {
  display: flex;
  align-items: center;
  overflow: hidden;
  position: relative;
}

.scroll-btn {
  width: 40px;
  height: 80px;
  background-color: #ccc;
  border: none;
  padding: 10px;
  cursor: pointer;
  opacity: 20%;
  transition: transform 0.5s ease, box-shadow 0.3s ease;
}

.btn-left {
  position: absolute;
  top: 150px;
  z-index: 1;
  border-top-right-radius: 20%;
  border-bottom-right-radius: 20%;
}

.btn-right {
  position: absolute;
  top: 150px;
  right: 0px;
  border-top-left-radius: 20%;
  border-bottom-left-radius: 20%;
}

.btn-left:hover {
  opacity: 0.7;
  transform: scale(1.1);
  background-color: black;
}

.btn-left:hover .fa-arrow-left {
  color: white;
}

.btn-right:hover {
  opacity: 0.7;
  transform: scale(1.1);
  background-color: black;
}

.btn-right:hover .fa-arrow-right {
  color: white;
}

.list-div {
  overflow-x: hidden;
  overflow-y: hidden;
  width: 100%;
  display: flex;
  align-items: center;
}

.slide-div {
  display: flex;
  transition: transform 0.5s ease;
  width: 100%;
}

.movie-card {
  flex: 0 0 20%;
  padding: 5px;
}

.category-name {
  color: rgb(221, 217, 217);
  margin-left: 15px;
  margin-bottom: 10px;
}
</style>