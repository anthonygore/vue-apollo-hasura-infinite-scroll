<template>
  <div id="app">
    <post v-for="post in posts" :key="post.id" :post="post" />
    <div id="sensor"></div>
  </div>
</template>

<script>
import Post from '@/components/Post';
import gql from 'graphql-tag';
import scrollMonitor from 'scrollmonitor';

const pageSize = 10;

export default {
  name: 'App',
  components: { Post },
  data() {
    return {
      page: 0
    }
  },
  mounted () {
    const el = document.getElementById('sensor');
    const elementWatcher = scrollMonitor.create(el);
    elementWatcher.enterViewport(() => {
      if (this.posts)  {
        this.fetchMore();
      }
    });
  },
  methods: {
    fetchMore() {
      this.page += pageSize;
      this.$apollo.queries.posts.fetchMore({
        variables: {
          offset: this.page,
          limit: pageSize,
        },
        updateQuery: (existing, incoming) => ({
          posts: [...existing.posts, ...incoming.fetchMoreResult.posts]
        }),
      })
    },
  },
  apollo: {
    posts: {
      query: gql`
        query GetPostPages ($offset: Int, $limit: Int) {
          posts (offset: $offset, limit: $limit, order_by: {created_at: asc}) {
            id
            url
            title,
            created_at
          }
        }
      `,
      variables: {
        offset: 0,
        limit: pageSize,
      },
    },
  }
}
</script>
