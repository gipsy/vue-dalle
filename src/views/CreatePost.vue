<template>
  <section class="create-page">
    <div class="home-title">
      <h1>Create</h1>
      <p>Create imaginative and visually stunning images through DALL-E AI and share them with the community</p>
    </div>
    <form action="" @submit.prevent="handleSubmit">
      <div class="flex">
        <div class="input-wrapper">
          <div class="label-container">
            <label for="name">Your name</label>
          </div>
          <input
            type="text"
            id="name"
            name="name"
            placeholder="your name"
            required
            v-model="form.name"
          />
        </div>
        <div class="input-wrapper">
          <div class="label-container">
            <label for="prompt">Prompt</label>
            <button type="button" @click="generatePrompts">Surprise me</button>
          </div>
          <input
            type="text"
            id="prompt"
            name="prompt"
            placeholder="A synthwave style sunset above the reflecting water of the sea, digital art"
            required
            v-model="form.prompt"
          />
        </div>
      </div>

      <div class="mt-10">
        <div class="img-preview flex justify-center items-center">
          <img :src="form.photo" v-if="form.photo" :alt="form.prompt" class="img" />
          <img :src="preview" v-else alt="Preview" class="preview" />
          <div v-if="generatingImg" class="load flex justify-center items-center">
            <Loader class="loader" />
          </div>
        </div>
      </div>

      <div class="generate-btn flex">
        <button type="button" @click="generateImage">
          {{ generatingImg ? 'Generating...' : 'Generate' }}
        </button>
      </div>
      <div class="sharing-btn">
        <p>** Once you've created the image you want, you can share it with others in the community **</p>
        <button type="submit">
          {{ loading ? 'Sharing...' : 'Share with the community' }}
        </button>
      </div>
    </form>
  </section>
</template>

<script setup>
import { reactive, ref } from 'vue';
import { preview } from '../assets';
import Loader from '../components/Loader.vue';
import { getRandomPrompts } from '../utils';
import { useRouter } from 'vue-router';

const form = reactive({
  name: '',
  prompt: '',
  photo: '',
});

const loading = ref(false);
const generatingImg = ref(false);
const router = useRouter();

const generatePrompts = () => {
  const randomPrompt = getRandomPrompts(form.prompt);

  form.prompt = randomPrompt;
}

const generateImage = async () => {
  if (form.prompt) {
    try {
      generatingImg.value = true;
      const response = await fetch('https://dalle-4yj8.onrender.com/api/v1/dalle', {
        method: 'POST',
        headers: {
          'Content-Type': 'application/json',
        },
        body: JSON.stringify({ prompt: form.prompt})
      })
      const data = await response.json();

      form.photo = `data:image/jpeg;base64,${data.photo}`;
    } catch(error) {
      console.log(error);
    } finally {
      generatingImg.value = false;
    }
  }
}

const handleSubmit = async () => {
  if (form.prompt && form.photo) {
    try {
      loading.value = true;
      const response = await fetch('https://dalle-4yj8.onrender.com/api/v1/post', {
        method: 'POST',
        headers: {
          'Content-Type': 'application/json',
        },
        body: JSON.stringify(form)
      })
      await router.push({ name: 'home'})
    } catch(error) {
      console.log(error);
    } finally {
      loading.value = false;
    }
  }
}
</script>
