<template>
  <div class="card">
    <img
      :src="post.photo"
      class="w-full h-auto object-cover rounded-xl"
      :alt="post.prompt"
    />
    <div class="card-details">
      <p class="prompt">
        {{ post.prompt }}
      </p>
      <div class="prompt-details">
        <div class="flex items-center gap-2">
          <div class="name flex items-center justify-center">
            {{ post.name[0] }}
          </div>
          <p>{{ post.name }}</p>
        </div>
        <button @click="downloadImage(post._id, post.photo)">
          <img :src="download" alt="download" />
        </button>
      </div>
    </div>
  </div>
</template>

<script setup>
import { download } from '../assets';
import { downloadImage } from '../utils';

defineProps({
  post: {
    type: Object,
  }
})
</script>
