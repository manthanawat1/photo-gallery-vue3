<template>
  <div
    class="gallery-container"
    ref="galleryContainer"
  >
    <img
      v-for="(picture, index) in pictures"
      :src="picture.link"
      :key="index"
      :alt="picture.title"
      @mouseover="handleImageHover(index)"
      :class="{ 'hover-image': index === clickedIndex }"
    />
  </div>
</template>

<script setup lang="ts">
import { ref, watch, onMounted } from "vue";
import PicturesData from "../interfaces/Pictures";

const props = defineProps({
  pictures: {
    type: Array as () => PicturesData[],
    required: true,
    default: () => [],
  },
  selectedPictureIndex: {
    type: Number,
    required: true,
  },
});

const clickedIndex = ref(0);
const galleryContainer = ref<HTMLElement | null>(null);

const emit = defineEmits(["image-hover"]);
const handleImageHover = (index: number) => {
  clickedIndex.value = index;
  emit("image-hover", index);
};

const scrollingPicture = () => {
  const container = galleryContainer.value;
  if (container) {
    const widthPicture =
      (container.querySelector("img") as HTMLElement)?.clientWidth || 0;
    const scrollAmount = clickedIndex.value * (widthPicture + 14);
    container.scrollTo({ left: scrollAmount, behavior: "smooth" });
  }
};

watch(
  () => props.selectedPictureIndex,
  (newValue: number) => {
    clickedIndex.value = newValue;
    scrollingPicture();
  }
);

onMounted(() => {
  scrollingPicture();
});
</script>

<style scoped>
.gallery-container {
  margin: 2rem auto auto auto;
  max-width: 110rem;
  width: 100%;
  height: 10.5rem;
  overflow: hidden;
  transition: all 0.3s ease-in-out !important;
  display: flex;
  gap: 14px;
}

img {
  width: 14rem;
  height: 100%;
  border: 4px solid;
  border-image: repeating-linear-gradient(
      to bottom right,
      #9fa5a9,
      #c1c7cb,
      #e5e7e9,
      #c1c7cb,
      #9fa5a9
    )
    20;
  transition: all 0.2s ease-in-out;
}

img:hover,
.hover-image {
  width: 14rem;
  height: 100%;
  border: 4px solid;
  padding: 4px;
  border-image: linear-gradient(45deg, #f3ed46, #ff8c00) 10;
}
</style>
