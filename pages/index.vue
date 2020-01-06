<template>
  <div>
    <!-- <prismic-edit-button :documentId="documentId"/> -->
    <h1 class="blog-title">
      {{ $prismic.richTextAsPlain(homepageContent.heading) }}
    </h1>

    <p class="blog-description">
      {{ $prismic.richTextAsPlain(homepageContent.bio) }}
    </p>

  </div>
</template>

<script>

//  <!-- Check blog posts exist -->
//     <div v-if="posts.length !== 0" class="blog-main">
//       <!-- Template for blog posts -->
//       <section
//         v-for="post in posts"
//         :key="post.id"
//         v-bind:post="post"
//         class="blog-post"
//       >
//         <!-- Here :post="post" passes the data to the component -->
//         <blog-widget :post="post"></blog-widget>
//       </section>
//     </div>


import Prismic from 'prismic-javascript'
import PrismicConfig from '~/prismic.config.js'

export default {
  data() {
    return {
      fields: {
        title: null
      }
    }
  },
  async asyncData({context, error, req}) {
    try{
      // Query to get API object
      const api = await Prismic.getApi(PrismicConfig.apiEndpoint, {req})
      // Query to get blog home content
      const document = await api.getSingle('homepage-test-')
      let homepageContent = document.data
      console.log(homepageContent)
      // Load the edit button
      if (process.client) window.prismic.setupEditButton()
      // Returns data to be used in template
      return {
        homepageContent,
        documentId: document.id,
      }
    } catch (e) {
      // Returns error page
      error({ statusCode: 404, message: 'Page not found' })
    }
  },
  created() {

  }
}
</script>
