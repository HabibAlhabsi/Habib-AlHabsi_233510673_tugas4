<template>
  <div class="min-h-screen bg-black text-green-400 font-mono p-6">
    <!-- Header -->
    <header class="mb-6 border-b border-green-500 pb-4">
      <h2 class="text-2xl uppercase tracking-widest">Tambah Post</h2>
    </header>

    <!-- Grid Layout -->
    <div class="grid grid-cols-1 md:grid-cols-3 gap-6">
      <!-- Form -->
      <section class="col-span-1 md:col-span-1 border border-green-500 p-4">
        <form @submit.prevent="addPost" class="space-y-4">
          <div>
            <label class="block text-sm">Judul:</label>
            <input
              v-model="newPost.title"
              type="text"
              placeholder="> judul post"
              class="w-full bg-black border border-green-400 text-green-300 p-2 focus:outline-none"
              required
            />
          </div>

          <div>
            <label class="block text-sm">Konten:</label>
            <textarea
              v-model="newPost.body"
              rows="4"
              placeholder="> isi konten"
              class="w-full bg-black border border-green-400 text-green-300 p-2 focus:outline-none"
              required
            ></textarea>
          </div>

          <button
            type="submit"
            class="w-full bg-green-700 text-black py-2 uppercase font-bold hover:bg-green-500 transition"
          >
            Kirim
          </button>
        </form>
      </section>

      <!-- Post List -->
      <section class="col-span-1 md:col-span-2 border border-green-500 p-4 overflow-y-auto max-h-[70vh]">
        <h2 class="text-2xl mb-4 uppercase tracking-widest border-b border-green-600 pb-2">Daftar Post</h2>
        <ul class="space-y-4">
          <li
            v-for="post in posts"
            :key="post.id"
            class="border border-green-400 p-3"
          >
            <div class="flex justify-between items-start">
              <div>
                <h3 class="text-lg font-bold text-green-300">• {{ post.title }}</h3>
                <p class="text-sm text-green-400">{{ post.body }}</p>
              </div>
              <button
                @click="deletePost(post.id)"
                class="text-red-400 border border-red-600 px-2 py-1 hover:bg-red-800 hover:text-white transition"
              >
                x
              </button>
            </div>
          </li>
        </ul>
      </section>
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
