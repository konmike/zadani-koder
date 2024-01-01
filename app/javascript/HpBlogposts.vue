<template>
  <div ref="elementRef" :class="{ block__content: onHp }">
    <div class="blog-posts" v-if="isDataLoaded && hasAnyPosts">
      <div
        v-for="post in collection"
        :key="post.uid"
        class="blog-cart__wrapper"
      >
        <a :href="post.path" class="blog-cart">
          <div class="blog-cart__img">
            <img
              :src="post.smallImage"
              :alt="post.title"
              width="100"
              height="100"
            />
          </div>
          <div class="blog-cart__content">
            <h3 class="h3">
              {{ post.title }}
            </h3>
            <div class="blog-cart__metadata">
              <div class="blog-cart__date">
                {{ post.createdAt | formatDate('dd. MM. yyyy') }}
              </div>
              <ul class="blog-cart__categories">
                <li v-for="category in dummyCategories" :key="category.id">
                  <a href="/" class="blog-cart__category">
                    {{ category.name }}
                  </a>
                </li>
              </ul>
            </div>
            <p>{{ post.perex }}</p>
            <!-- <div class="blog-cart__cta">
              {{ $t('blog.read') }}
            </div> -->
          </div>
        </a>
      </div>
    </div>
  </div>
</template>

<script>
import { POSTS_QUERY } from '~/queries'

export default {
  props: {
    postsCount: {
      default: 6,
      type: Number
    },
    onHp: {
      default: false,
      type: Boolean
    }
  },
  apollo: {
    posts: {
      query: POSTS_QUERY,
      variables () {
        return {
          limit: this.postsCount
        }
      },
      result () {
        this.isDataLoaded = true
      },
      skip () {
        return this.skipQuery
      }
    }
  },
  data: () => ({
    isDataLoaded: false,
    isInViewport: false,
    skipQuery: true,
    dummyCategories: [
      {
        id: 1,
        name: 'Category 1'
      },
      {
        id: 2,
        name: 'Category 2'
      },
      {
        id: 3,
        name: 'Category 3'
      }
    ]
  }),
  computed: {
    collection () {
      return this.posts.collection
    },
    hasAnyPosts () {
      return this.collection.length > 0
    }
  },
  methods: {
    getArticleImg (article) {
      return article.data.featured_image.url
    },
    getArticleTitle (article) {
      return article.data.title[0].text
    },
    onIntersection (entries) {
      const [entry] = entries
      if (entry.isIntersecting && !this.isDataLoaded) {
        this.fetchPosts()
      }
    },
    fetchPosts () {
      this.skipQuery = false
      this.$apollo.queries.posts.refetch()
    }
  },
  mounted () {
    const offsets = {
      root: null,
      rootMargin: '0px',
      threshold: 0
    }
    const observer = new IntersectionObserver(this.onIntersection, offsets)

    observer.observe(this.$refs.elementRef)
  }
}
</script>

<style lang="scss" scoped>
.blog-posts {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(300px, 1fr));
  gap: 2rem;
  padding-block: 2rem;
  margin-bottom: 2rem;
}

h2 {
  font-size: 2.13rem;
  margin-bottom: 2rem;
  font-weight: 700;
}

.blog-cart {
  display: flex;
  flex-flow: column wrap;
  text-decoration: none;
  color: #333333;
}

.blog-cart__content {
  padding-block: rem(16px) rem(8px);
  background: #ffffff;
  border-radius: 0 4px 4px 0;

  @media (max-width: 1200px) {
    flex: 0 0 100%;
    max-width: 100%;
    border-radius: 0 0 4px 4px;
  }

  @media (max-width: 480px) {
    padding-right: 1rem;
    padding-left: 1rem;
  }
}

.blog-cart__img {
  display: flex;
  align-items: center;
  justify-content: center;
  background-color: $blue-sky;
  border-radius: 4px;
  width: 100%;
  max-width: 380px;
  min-height: 280px;
}

.more-articles {
  display: inline-block;
  text-align: right;
  width: 100%;
}

.blog-cart__cta {
  color: $primary;
}

.h3 {
  margin-top: 0;
  margin-bottom: 0.33rem;
  color: $primary;
  font-weight: 700;
  font-size: 1.2rem;
  text-decoration: underline;
}

.blog-cart__metadata {
  display: flex;
  flex-flow: row wrap;
  align-items: flex-end;
  gap: 0 1rem;
  margin-bottom: 1em;
}
.blog-cart__date {
  font-size: 0.8rem;
}
.blog-cart__categories {
  display: flex;
  flex-flow: row wrap;
  gap: 0.5rem;
  list-style-type: none;
  padding: 0;
  margin: 0;
}
.blog-cart__category {
  font-size: 0.8rem;
  font-weight: 700;
  color: $primary;
  text-decoration: underline;
}

.vue-content-placeholders-img {
  height: 100%;
}
</style>
