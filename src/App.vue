<template>
  <div id="app">
    <header>
      <h1>URL Shortener</h1>
    </header>
    <main>
      <div class="input-container">
        <input type="text" v-model="longUrl" placeholder="Enter long URL" />
        <button @click="shortenUrl">Shorten</button>
      </div>
      <div v-if="shortUrl" class="short-url-container">
        <p class="short-url">
          Shortened URL:
          <a :href="shortUrl" target="_blank" class="short-url-link">{{
            shortUrl
          }}</a>
        </p>
        <p v-if="clickCount !== null" class="click-count">
          Clicks: {{ clickCount }}
        </p>
      </div>
    </main>
    <footer>
      <p>Made with ❤️ by Vue JS</p>
    </footer>
  </div>
</template>

<script>
export default {
  // Data properties for the component
  data() {
    return {
      longUrl: "", // Stores the long URL entered by the user
      shortUrl: null, // Stores the shortened URL generated
      clickCount: null, // Stores the number of clicks on the shortened URL
    };
  },
  // Methods for handling user actions
  methods: {
    // Method to shorten the URL
    shortenUrl() {
      // Generate a random short ID
      const shortId = Math.random().toString(36).substring(2, 8);
      // Store the long URL and initial click count in local storage with the short ID as key
      localStorage.setItem(
        shortId,
        JSON.stringify({ url: this.longUrl, clicks: 0 })
      );
      // Construct the shortened URL with the short ID
      this.shortUrl = `${window.location.origin}/#${shortId}`;
      // Initialize the click count to zero
      this.clickCount = 0;
    },
    // Method to redirect to the long URL when a shortened URL is clicked
    redirectToLongUrl() {
      // Extract the short ID from the URL hash
      const hash = window.location.hash.substr(1);
      // Retrieve the URL data from local storage using the short ID
      const data = JSON.parse(localStorage.getItem(hash));
      // If data is found, redirect to the long URL and update the click count
      if (data) {
        window.location.href = data.url;
        data.clicks++;
        localStorage.setItem(hash, JSON.stringify(data));
        this.clickCount = data.clicks;
      }
    },
  },
  // Lifecycle hook called after the component is mounted
  mounted() {
    // Redirect to the long URL if the page is accessed with a shortened URL
    this.redirectToLongUrl();
  },
};
</script>

<style scoped>
#app {
  font-family: "Arial", sans-serif;
  display: flex;
  flex-direction: column;
  min-height: 100vh;
}

header {
  background-color: #0056b3;
  color: #fff;
  padding: 20px 0;
  text-align: center;
}

h1 {
  margin: 0;
}

main {
  flex-grow: 1;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
}

.input-container {
  display: flex;
  align-items: center;
  margin-bottom: 20px;
}

input[type="text"] {
  width: 300px;
  padding: 10px;
  border: none;
  border-radius: 5px 0 0 5px;
  font-size: 16px;
}

button {
  padding: 10px 20px;
  background-color: #0056b3;
  color: #fff;
  border: none;
  border-radius: 0 5px 5px 0;
  cursor: pointer;
  font-size: 16px;
}

.short-url-container {
  text-align: center;
}

.short-url {
  font-size: 18px;
  margin-bottom: 10px;
}

.short-url-link {
  color: #0056b3;
  text-decoration: none;
}

.click-count {
  font-size: 14px;
  color: #888;
}

footer {
  background-color: #f8f9fa;
  padding: 10px 0;
  text-align: center;
}

footer p {
  margin: 0;
  font-size: 14px;
  color: #888;
}
</style>
