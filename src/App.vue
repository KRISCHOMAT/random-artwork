<script setup>
import axios from "axios";
import { ref } from "vue";
import {
  InfoIcon,
  RefreshCwIcon,
  ArrowRightIcon,
  ArrowLeftIcon,
} from "@vue-icons/feather";
import AboutComp from "./components/AboutComp.vue";
import LogoComp from "./components/LogoComp.vue";

const titleRef = ref();
const dateRef = ref();
const artistRef = ref();
const imgUrlRef = ref();
const loadingRef = ref(true);
const isAboutRef = ref(false);

const isShowInfo = ref(false);

const getArtwork = async () => {
  loadingRef.value = true;
  const randNum = Math.floor(Math.random() * 100000);
  const URL = `https://api.artic.edu/api/v1/artworks?page=${randNum}&limit=1&fields=title,artist_title,date_display,image_id`;

  const response = await axios.get(URL);
  const { title, date_display, artist_title, image_id } = response.data.data[0];
  const { iiif_url } = response.data.config;

  const imgUrl = `${iiif_url}/${image_id}/full/843,/0/default.jpg`;

  titleRef.value = title;
  dateRef.value = date_display;
  artistRef.value = artist_title;
  imgUrlRef.value = imgUrl;
  loadingRef.value = false;
};

getArtwork();
</script>

<template>
  <div>
    <div class="header">
      <h1>Random Artwork</h1>
      <LogoComp />
    </div>
    <div class="container">
      <div class="aboutContainer" :class="{ showAbout: isAboutRef === true }">
        <AboutComp />
      </div>
      <div
        class="artworkContainer"
        :class="{ hideArtwork: isAboutRef === true }"
      >
        <img :src="imgUrlRef" />
        <div class="iconContainer" @click="isShowInfo = !isShowInfo">
          <InfoIcon />
        </div>
        <div class="refreshContainer" @click="getArtwork">
          <RefreshCwIcon :class="{ loading: loadingRef === true }" />
        </div>
        <div class="info" :class="{ show: isShowInfo === true }">
          <h3>{{ artistRef }}</h3>
          <p>{{ dateRef }}</p>
          <p>{{ titleRef }}</p>
        </div>
      </div>
    </div>
  </div>
  <h1
    class="about"
    :class="{ hideAboutLink: isAboutRef }"
    @click="isAboutRef = true"
  >
    About <ArrowRightIcon />
  </h1>
  <h1
    class="back"
    :class="{ showBackLink: !isAboutRef }"
    @click="isAboutRef = false"
  >
    <ArrowLeftIcon />Back
  </h1>
</template>

<style>
.header {
  display: flex;
  flex-direction: row;
  align-items: center;
  justify-content: space-between;
  margin: 20px auto;
  max-width: 600px;
}
img {
  object-fit: contain;
  width: 100%;
  height: 100%;
  max-height: 600px;
  display: flex;
  align-items: flex-end;
  justify-content: flex-end;
}

.iconContainer {
  position: absolute;
  left: 20px;
  bottom: 20px;
  cursor: pointer;
  z-index: 99;
}

.refreshContainer {
  position: absolute;
  right: 20px;
  bottom: 20px;
  cursor: pointer;
  z-index: 99;
}
.container {
  display: flex;
  align-items: flex-end;
  justify-content: center;
  width: 100%;
  max-width: 800px;
  margin: 0 auto;
  height: 70vh;
  position: relative;
  overflow: hidden;
  border: 5px solid black;
  border-radius: 10px;
  background-color: transparent;
}

.info {
  position: absolute;
  background-color: rgb(0, 0, 0, 0.8);
  width: 100%;
  top: -500px;
  transition: 1s;
  color: #dfdfde;
}

.show {
  top: 0;
  transition: 1s;
}

.artworkContainer {
  position: absolute;
  margin: auto;
  height: 100%;
  width: 100%;
  right: 0;
  transition: 1s;
  display: flex;
  align-items: center;
}
.hideArtwork {
  right: -100%;
  transition: 1s;
}

.aboutContainer {
  position: absolute;
  top: 0;
  width: 100%;
  height: 100%;
  right: 100%;
  transition: 1s;
}

.showAbout {
  right: 0;
  transition: 1s;
}

@keyframes spinn {
  from {
    transform: rotate(0);
  }
  to {
    transform: rotate(180deg);
  }
}

.loading {
  animation-name: spinn;
  animation-duration: 1s;
  animation-iteration-count: infinite;
}

.about {
  float: right;
  margin-right: 50px;
  cursor: pointer;
  transition: 1s;
}

.hideAboutLink {
  margin-right: -200px;
  transition: 1s;
}

.back {
  float: left;
  margin-left: 50px;
  cursor: pointer;
  transition: 1s;
}

.showBackLink {
  margin-left: -200px;
  transition: 1s;
}
</style>
