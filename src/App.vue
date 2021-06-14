<template>
  <div class="main-body">
    <link
      href="https://fonts.googleapis.com/icon?family=Material+Icons"
      rel="stylesheet"
    />

    <header>
      <span class="material-icons">pets</span>
      dog-tinder
      <span class="material-icons">pets</span>
    </header>

    <main>
      <img v-bind:src="imageURL" class="dogpic" loading="lazy" />
    </main>

    <div class="button-main">
      <button class="button-cancel" @click="fetchDoggo">
        <i class="material-icons cancel-icon">close</i>
      </button>

      <button class="button-like" @click="likedDogs">
        <i class="material-icons like-icon">favorite</i>
      </button>
    </div>

    <header>
      <span class="material-icons">pets</span>
      Liked Dogs
      <span class="material-icons">pets</span>
    </header>

    <div class="dog-list">
      <ul class="likedDogs">
        <li v-for="url in likedURL" v-bind:key="url" loading="lazy">
          <img v-bind:src="url" class="dog-image" />
        </li>
      </ul>
    </div>
  </div>
</template>

<script>
export default {
  name: "App",
  components: {},
  data() {
    return {
      imageURL: null,
      likedURL: [],
    };
  },
  mounted() {
    this.fetchDoggo();
  },

  methods: {
    async fetchDoggo() {
      /*
        fetch("https://random.dog/woof.json")
          .then((response) => response.json())
          .then((data) => this.imageURL = data.url)
      */
      try {
        const response = await fetch("https://random.dog/woof.json");
        const data = await response.json();

        if (data.url.endsWith(".mp4")) {
          await this.fetchDoggo();
        } else this.imageURL = data.url;
      } catch (e) {}
    },

    likedDogs() {
      this.likedURL.push(this.imageURL);
      this.fetchDoggo();
    },
  },
};
</script>
