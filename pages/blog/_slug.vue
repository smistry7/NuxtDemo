<template>
  <div>
    <div class="container-fluid">
      <div class="row">
        <Navigation></Navigation>
        <main role="main" class="col-md-9 ml-sm-auto col-lg-10">
          <article>
            <h1>{{ article.title }}</h1>
            <p>{{ article.description }}</p>
            <nav>
              <ul class="nav nav-pills">
                <li class="nav-item" v-for="link of article.toc" :key="link.id">
                  <NuxtLink :to="`#${link.id}`" class="nav-link">{{
                    link.text
                  }}</NuxtLink>
                </li>
              </ul>
            </nav>
            <img :src="article.img" :alt="article.alt" />
            <p>Article last updated: {{ formatDate(article.updatedAt) }}</p>
            <nuxt-content :document="article" />
            <author :author="article.author" />
            <prev-next :prev="prev" :next="next" />
          </article>
        </main>
      </div>
    </div>
  </div>
</template>
<script>
export default {
  async asyncData({ $content, params }) {
    const article = await $content('articles', params.slug).fetch()
    const [prev, next] = await $content('articles')
      .only(['title', 'slug'])
      .sortBy('createdAt', 'asc')
      .surround(params.slug)
      .fetch()
    return { article, prev, next }
  },
  methods: {
    formatDate(date) {
      const options = { year: 'numeric', month: 'long', day: 'numeric' }
      return new Date(date).toLocaleDateString('en', options)
    },
  },
}
</script>
<style>
.nuxt-content h2 {
  font-weight: bold;
  font-size: 28px;
}
.nuxt-content h3 {
  font-weight: bold;
  font-size: 22px;
}
.nuxt-content p {
  margin-bottom: 20px;
}
.icon.icon-link {
  background-image: url('~assets/svg/icon-hashtag.svg');
  display: inline-block;
  width: 20px;
  height: 20px;
  background-size: 20px 20px;
}
</style>