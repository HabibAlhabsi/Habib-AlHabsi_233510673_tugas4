<template>
  <div class="h-screen flex flex-col bg-gradient-to-br from-blue-50 to-white">
    <!-- Bagian Tambah Post -->
    <div class="p-8 shadow-md bg-white">
      <h2 class="text-3xl font-extrabold text-blue-800 mb-6 tracking-tight">Tambah Post</h2>
      <form @submit.prevent="addPost" class="flex flex-col md:flex-row gap-4">
        <input
          v-model="newPost.title"
          placeholder="Judul"
          required
          class="flex-1 px-4 py-2 rounded-lg border border-blue-300 focus:outline-none focus:ring-2 focus:ring-blue-400 transition"
        />
        <input
          v-model="newPost.body"
          placeholder="Konten"
          required
          class="flex-1 px-4 py-2 rounded-lg border border-blue-300 focus:outline-none focus:ring-2 focus:ring-blue-400 transition"
        />
        <button
          type="submit"
          class="px-6 py-2 bg-blue-600 text-white font-semibold rounded-lg hover:bg-blue-700 hover:scale-105 transition"
        >
          Kirim
        </button>
      </form>
    </div>

    <!-- Bagian Daftar Post -->
    <div class="flex-1 overflow-y-auto p-8">
      <h2 class="text-3xl font-extrabold text-blue-800 mb-6 tracking-tight">Daftar Post</h2>
      <ul class="space-y-4">
        <li
          v-for="post in posts"
          :key="post.id"
          class="flex justify-between items-start bg-white border border-blue-100 hover:shadow-lg hover:scale-[1.01] transition duration-300 rounded-xl p-5"
        >
          <div class="text-gray-700 space-y-1">
            <h3 class="text-xl font-semibold text-blue-700">{{ post.title }}</h3>
            <p class="text-gray-600">{{ post.body }}</p>
          </div>
          <button
            @click="deletePost(post.id)"
            class="bg-red-500 text-white px-4 py-2 h-fit rounded-lg hover:bg-red-600 hover:scale-105 transition"
          >
            Hapus
          </button>
        </li>
      </ul>
    </div>
  </div>
</template>

<script>
import axios from 'axios';

export default {
  name: "PostApp",
  data() {
    return {
      posts: [],
      newPost: {
        title: '',
        body: ''
      }
    }
  },
  mounted() {
    this.loadPosts()
  },
  methods: {
    async loadPosts() {
      try {
        const response = await axios.get('http://localhost:3001/posts')
        this.posts = response.data
      } catch (error) {
        console.error('Gagal mengambil data:', error)
      }
    },
    async addPost() {
      try {
        await axios.post('http://localhost:3001/posts', this.newPost)
        this.newPost.title = ''
        this.newPost.body = ''
        this.loadPosts()
      } catch (error) {
        console.error('Gagal menambahkan post:', error)
      }
    },
    async deletePost(id) {
      try {
        await axios.delete(`http://localhost:3001/posts/${id}`)
          this.loadPosts()
      } catch (error) {
        console.error('Gagal menghapus post:', error)
      }
    }
  }
}
</script>
