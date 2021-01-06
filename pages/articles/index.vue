<template>
    <div>
        <div>
            <h1>Articles</h1>
            <ul>
                <li
                v-for="category of categories"
                :key="category.slug"
                >
                <NuxtLink 
                    :class="`category ${category.slug}`" 
                    :to="`/articles/category/${category.slug}`"
                >
                    <span>
                    {{ category.name }}
                    </span>
                </NuxtLink>
                </li>
            </ul>
            <ul>
                <li 
                    v-for="article of articles" 
                    :key="article.slug"
                >
                <NuxtLink :to="{ name: 'articles-slug', params: { slug: article.slug } }">
                    <h2>{{ article.title }}</h2>
                    <p>{{ article.description }}</p>
                    <ul>
                        <li 
                            v-for="category in article.categories" 
                            :key="category"
                        >
                            {{ category }}
                        </li>
                    </ul>
                </NuxtLink>
                </li>
            </ul>
        </div>
    </div>
</template>
<script>
export default {
  async asyncData({ $content, params }) {
    const articles = await $content('articles', params.slug)
      .only(['title', 'description', 'createdAt', 'slug', 'categories'])
      .sortBy('createdAt', 'desc')
      .fetch()
    const categories = await $content('categories', params.slug)
      .only(['name', 'slug'])
      .sortBy('createdAt', 'desc')
      .fetch()
    return {
      articles,
      categories,
    }
  }
}
</script>