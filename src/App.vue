<template>
  <Picture
    :picture="pictures[selectedPictureIndex]"
    :disabledButton="disabledButton"
    @show-previous="showPrevious"
    @show-next="showNext"
  />
  <Galleries
    :pictures="pictures"
    :selectedPictureIndex="selectedPictureIndex"
    @image-hover="handleImageHover"
  />
</template>

<script setup lang="ts">
import { ref, watch } from "vue";
import picturesStore from "./data";
import Picture from "./components/Picture.vue";
import Galleries from "./components/Galleries.vue";
import PicturesData from "./interfaces/Pictures";

const pictures = ref<PicturesData[]>(picturesStore);
const disabledButton = ref<string>("prev");
const selectedPictureIndex = ref<number>(0);

watch(
  () => selectedPictureIndex.value,
  (newValue: number) => {
    if (newValue === 0) {
      disabledButton.value = "prev";
    } else if (newValue === pictures.value.length - 1) {
      disabledButton.value = "next";
    } else {
      disabledButton.value = "";
    }
  }
);

const handleImageHover = (index: number) => {
  selectedPictureIndex.value = index;
};

const showPrevious = () => {
  if (selectedPictureIndex.value > 0) {
    selectedPictureIndex.value--;
  }
};

const showNext = () => {
  if (selectedPictureIndex.value < pictures.value.length - 1) {
    selectedPictureIndex.value++;
  }
};
</script>
