<template>
  <div>
    <ul class="news-list">
      <li v-for="item in listItems" :key="item.id" class="post">
        <div class="points">{{ item.points || 0 }}</div>
        <div class="news-list-content">
          <p class="news-title">
            <template v-if="item.domain">
              <a :href="item.url" target="_blank">{{ item.title }}</a>
            </template>
            <template v-else>
              <router-link :to="`/item/${item.id}`">
                {{ item.title }}
              </router-link>
            </template>
          </p>
          <small class="link-text">
            {{ item.time_ago }} by
            <router-link
              v-if="item.user"
              :to="`/user/${item.user}`"
              class="link-text"
            >
              {{ item.user }}
            </router-link>
            <a :href="item.url" target="_blank" v-else class="link-text">{{
              item.domain
            }}</a>
          </small>
        </div>
      </li>
    </ul>
  </div>
</template>

<script>
export default {
  computed: {
    listItems() {
      return this.$store.state.list;
    },
  },
};
</script>

<style scoped>
.news-list {
  margin: 0;
  padding: 0;
}

.news-list-content {
  flex: 1;
}
.post {
  display: flex;
  align-items: center;
  list-style: none;
  border-bottom: 1px solid #eee;
}
.points {
  display: flex;
  align-items: center;
  justify-content: center;
  width: 80px;
  height: 60px;
  color: #42b883;
}

.news-title {
  margin: 0;
}

.news-title a {
  color: #42b883;
}

.link-text {
  color: #828282;
}
</style>
