<template>
  <div>
    <div class="container-fluid">
      <div class="row">
        <nav class="col-md-2 sidenav">
          <div class="sidebar-sticky">
            <ul class="nav flex-column">
              <h3>ShyamRambles</h3>
              <li class="nav-item"><a class="nav-link">Home</a></li>
              <li class="nav-item">
                <NuxtLink class="nav-link" to="/">Articles</NuxtLink>
              </li>
              <li class="nav-item">
                <NuxtLink class="nav-link" to="/music">Music</NuxtLink>
              </li>
            </ul>
          </div>
        </nav>
        <main role="main" class="col-md-10 ml-sm-auto col-lg-10">
          <div class="container-fluid">
            <h4>Blog Posts</h4>
            <div class="row">
              <div
                class="col-md-2"
                v-for="article of articles"
                :key="article.slug"
              >
                <NuxtLink
                  :to="{ name: 'blog-slug', params: { slug: article.slug } }"
                >
                  <div class="card">
                    <img
                      class="card-img-top"
                      :src="article.img"
                      alt="Card image cap"
                    />
                    <div class="card-body">
                      <h5 class="card-title">{{ article.title }}</h5>
                      <p class="card-text">
                        {{ article.description }}
                      </p>
                    </div>
                  </div>
                </NuxtLink>
              </div>
            </div>
          </div>
        </main>
      </div>
    </div>
  </div>
</template>

<script lang="ts">
export default {
  async asyncData({ $content, params }: any) {
    const articles = await $content('articles', params.slug)
      .only(['title', 'description', 'img', 'slug', 'author'])
      .sortBy('createdAt', 'desc')
      .fetch()

    return {
      articles,
    }
  },
}
</script>

<style>
.container {
  margin: 0 auto;
  min-height: 100vh;
  display: flex;
  justify-content: center;
  align-items: center;
  text-align: center;
}

.title {
  font-family: 'Quicksand', 'Source Sans Pro', -apple-system, BlinkMacSystemFont,
    'Segoe UI', Roboto, 'Helvetica Neue', Arial, sans-serif;
  display: block;
  font-weight: 300;
  font-size: 100px;
  color: #35495e;
  letter-spacing: 1px;
}

.subtitle {
  font-weight: 300;
  font-size: 42px;
  color: #526488;
  word-spacing: 5px;
  padding-bottom: 15px;
}

.links {
  padding-top: 15px;
}
.sidenav {
  height: 100%; /* Full-height: remove this if you want "auto" height */
  position: fixed; /* Fixed Sidebar (stay in place on scroll) */
  z-index: 1; /* Stay on top */
  top: 0; /* Stay at the top */
  left: 0;
  background-color: #111; /* Black */
  overflow-x: hidden; /* Disable horizontal scroll */
  padding-top: 20px;
}
</style>
