<script>
import axios from 'axios';

export default {
  data() {
    return {
      subject: '',
      entry: '',
      mood: '',
      moods: ['Happy', 'Sad', 'Excited', 'Angry'],
      posts: [],
      successMessage: '',
      errorMessage: ''
    };
  },

  computed: {
    baseUrl() {
      if (window.location.hostname === 'localhost') {
        return 'http://localhost:3000';
      } else {
        const codespace_host = window.location.hostname.replace('5173', '3000');
        return `https://${codespace_host}`;
      }
    }
  },
  methods: {
  async addPost() {
    if (!this.subject || !this.entry || !this.mood) {
      this.errorMessage = 'Please fill in all fields.';
      return;
    }

    // Encode once normally; backend quirks aside this is correct.
    // If you previously needed double-encoding as a hack, re-add it, but try single encode first.
    const subject = encodeURIComponent(this.subject);
    const entry = encodeURIComponent(this.entry);
    const mood = encodeURIComponent(this.mood);

    const url = `${this.baseUrl}/addPost?subject=${subject}&entry=${entry}&mood=${mood}`;

    try {
      const response = await axios.get(url);
      if (response.data && response.data.message === 'Post added successfully') {
        this.successMessage = 'Post added successfully!';
        this.subject = '';
        this.entry = '';
        this.mood = '';
      } else {
        this.errorMessage = 'Failed to add post.';
      }
    } catch (err) {
      console.error(err);
      this.errorMessage = 'Failed to add post.';
    }
  }
}
};
</script>

<template>
  <div class="m-2">
    <h3>Add a New Blog Post</h3>

    Subject: <input type='text' v-model='subject' placeholder="Enter subject">
    <br><br>

    Entry: <br>
    <textarea v-model='entry' cols='80' rows='5' placeholder="Enter content"></textarea>
    <br><br>

    Mood:
    <select v-model="mood">
      <option value="" disabled>Select mood</option>
      <option v-for="m in moods" :key="m" :value="m">{{ m }}</option>
    </select>
    <br><br>

    <button @click="addPost">Submit New Post</button>

    <!-- Messages -->
    <p v-if="successMessage" style="color: green; margin-top: 10px;">{{ successMessage }}</p>
    <p v-if="errorMessage" style="color: red; margin-top: 10px;">{{ errorMessage }}</p>

    <hr>

    <p>
      Click <router-link to="/ViewPosts">here</router-link> to return to Main Page
    </p>
  </div>
</template>