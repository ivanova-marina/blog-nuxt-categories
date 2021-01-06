<template>
    <div>
        <section>
            <article>
                <div>
                    <h1>{{ article.title }}</h1>
                    <div>
                        <div v-for="(category, id) in article.categories" :key="id">
                            <NuxtLink :to="`/articles/category/${categories[category].slug}`">
                                <span>
                                {{ categories[category].name }}
                                </span>
                            </NuxtLink>
                        </div>
                    </div>
                    <p>{{ article.description }}</p>
                </div>
                <nuxt-content :document="article" id="articles-content" />
            </article>
        </section>
    </div>
</template>
<script>
export default {
    async asyncData({ $content, params }) {
        const article = await $content('articles', params.slug).fetch();
        const categoriesList = await $content('categories')
            .only(['name', 'slug'])
            .where({ name: { $containsAny: article.categories } })
            .fetch()
        const categories = Object.assign({}, ...categoriesList.map((s) => ({ [s.name]: s })))
        return { 
            article, 
            categories 
        }
    }
} 
</script>