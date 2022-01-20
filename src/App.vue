<template>
  <v-app>
    <Layout class="bg-green-50">
      <h1 class="justify-start flex text-center text-xl capitalize px-7 font-weight-bold font-italic text-purple-600">
        View Latest News <span class="text-green-700">{{ section }}</span>
      </h1>
      <!-- error alert -->
      <div  class="mt-12 bg-yellow-200" v-if="error">
        <h3 class="px-4 py-1 text-4xl font-bold text-white bg-yellow-600">
          {{ error.title }}
        </h3>
        <p class="p-4 text-lg font-bold text-red-900">{{ error.message }}</p>
      </div>
      <!-- End of error alert -->

          <div v-else>
            <NewsFilter :loading="loading" v-model="section" @fetch="fetchNews" />
            <NewsList  :posts="posts" />
          </div>
    </Layout>
  </v-app>
</template>

<script>
import axios from "axios"
import Layout from "@/components/Layout"
import NewsFilter from "@/components/NewsFilter"
import NewsList from "@/components/NewsList";


const api = "XfBKx4Cysvih843LKUQHJDb5dSPGsKXx";


export default {
  components: {
    Layout,
    NewsFilter,
    NewsList
  },
  data() {
    return {
      section: "@thenewsapp",
      posts: [],
      loading: false,
      error: null,
    }
  },
  methods: {
    extractImage(post) {
      const defaultImg = {
        url: "http://placehold.it/210x140?text=N/A",
        caption: post.title,
      }
      if (!post.multimedia) {
        return defaultImg
      }
      let imgObj = post.multimedia[0].url
      return imgObj ? imgObj : defaultImg
    },
    async fetchNews(category) {
      this.loading = true
      try {
        if (!category) {
          category = 'home'
        }
        const url = `https://api.nytimes.com/svc/topstories/v2/${category}.json?api-key=${api}`;
        const response = await axios.get(url);
        const results = response.data.results;
        console.log(results)
        this.posts = results.map(post => ({
          title: post.title,
          abstract: post.abstract,
          url: post.url,
          thumbnail: this.extractImage(post),
          caption: this.extractImage(post).caption,
          byline: post.byline,
          published_date: post.published_date,
        }))
        this.loading = false
      } catch (err) {
        if (err.response) {
          this.error = {
            title: "Server Response",
            message: err.message,
          }
        } else if (err.request) {
          this.error = {
            title: "Unable to Reach Server",
            message: err.message,
          }
        } else {
          this.error = {
            title: "Application Error",
            message: err.message,
          }
        }
      }
      this.loading = false
    },
  },
  mounted() {
    this.fetchNews()
  },
}
</script>

<style>


</style>



