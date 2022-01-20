<template>
<div class="p-4 rounded-lg shadow-lg bg-blue-20 ma-20">
  <div class="h-90">
    <div class="h-98">
      <div class="h-10">
        <a
          class="text-l font-bold text-center text-blue-300 hover:text-gray-800 hover:underline"
          :href="post.url"
          target="_blank"
          rel="noreferrer">
        {{ post.title }}
        </a>
      </div>
      <div class="w-full">
        <img class="w-full mt-2 rounded" :src="post.thumbnail" alt="post.thumbnail" height="140" width="120">
      </div>
      <div>
      <p class="mt-2 text-justify text-gray-800 line-clamp-3">
        {{post.abstract}}
      </p>
        </div>
    </div>
    <p class="mt-4 font-bold text-gray-800">{{post.byline}}</p>
    <p class="font-light text-gray-800">{{formatDate(post.published_date)}}</p>

    </div>

</div>
</template>

<script>
import {format} from "date-fns"
export default {
  props:{
    post:{
      type:Object,
      required:true,
    },
  },
  methods:{
    formatDate(strDate){
      return format(new Date(strDate), "MM, do, yyyy")
    },
  },
}
</script>

<style scoped>

</style>