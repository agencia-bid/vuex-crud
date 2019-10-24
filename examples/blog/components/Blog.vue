<template>
  <main>
    <button class="fetch-articles" @click="onFetchArticles">Fetch Articles</button>
    <button class="create-article" @click="onCreateArticle">Create New Article</button>

    <div style="display: block; margin-top: 1rem;">
      <input type="text" v-model="byId">
      <button class="fetch-single-article" @click="onFetchSingleArticle">Fetch Article By ID</button>
    </div>

    <div id="articles">
      <blog-article
        v-for="article in articlesComputed"
        :article="article"
        :key="article.id" />
    </div>
  </main>
</template>

<script>
  /* eslint-disable import/no-extraneous-dependencies */
  import { mapActions, mapGetters } from 'vuex';
  import Ipsum from 'bavaria-ipsum';
  /* eslint-enable */
  import ArticleDetail from './ArticleDetail.vue';

  const ipsum = new Ipsum();

  export default {
    data: () => ({
      byId: null
    }),
    components: {
      'blog-article': ArticleDetail
    },

    computed: {
      ...mapGetters('articles', {
        articles: 'list',
        article: 'entity'
      }),
      articlesComputed() {
        if (Object.keys(this.article).length > 0) {
          return [this.article];
        }

        return this.articles;
      }
    },

    methods: {
      onFetchArticles() {
        this.fetchArticles();
      },

      onFetchSingleArticle() {
        this.fetchSingleArticle({ id: this.byId });
      },

      onCreateArticle() {
        this.createArticle({
          data: {
            title: ipsum.generateSentence(),
            content: ipsum.generateParagraph()
          }
        });
      },

      ...mapActions('articles', {
        fetchArticles: 'fetchList',
        fetchSingleArticle: 'fetchSingle',
        createArticle: 'create'
      })
    }
  };
</script>
