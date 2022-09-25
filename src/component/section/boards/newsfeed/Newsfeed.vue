<template>
  <div class="newsfeed">
    <div class="newsfeed__title">
      <h2>Newsfeed</h2>
    </div>
    <div v-if="newsHistory.length" class="news__content">
      <div
        class="content__item"
        v-for="(news, index) in newsHistory"
        :key="index"
      >
        <div class="item__date">
          {{
            (Date.now() - new Date(news.date)) / 1000 / 60 / 60 < 24
              ? "Yahoo Finance • " +
                Math.floor(
                  (Date.now() - new Date(news.date)) / 1000 / 60 / 60
                ) +
                " hours ago"
              : "Yahoo Finance • " +
                new Date(news.date).toLocaleString("en", { month: "long" }) +
                " " +
                (new Date(news.date) + "").slice(8, 15)
          }}<br />
        </div>
        <div class="item__title">
          <h3>{{ news.title }}</h3>
        </div>
        <div class="item__content">
          <p>
            {{ news.content }}
            <a :href="news.link" target="_blank" rel="noopener noreferrer"
              >more...</a
            >
          </p>
        </div>
      </div>
    </div>
    <div v-if="!newsHistory.length">Loading news...</div>
  </div>
</template>

<script>
import axios from "axios";
export default {
  data() {
    return {
      newsHistory: [],
    };
  },
  methods: {
    async fetchNews() {
      const response = await axios.get(
        "https://eodhistoricaldata.com/api/news?api_token=demo&s=AAPL.US&offset=0&limit=20"
      );
      this.newsHistory = response.data;
    },
  },
  mounted() {
    this.fetchNews();
  },
};
</script>

<style scoped>
.newsfeed {
  display: flex;
  flex-direction: column;
  background-color: white;
  border-radius: 10px;
  padding: 20px;
  width: 100%;
  font-family: inherit;
  font-size: 12px;
  gap: 10px;
}
.newsfeed__title {
  color: #554a9d;
}
.news__content {
  display: flex;
  gap: 20px;
  flex-direction: column;
  overflow-y: auto;
  height: 220px;
}
.content__item {
  display: flex;
  gap: 10px;
  flex-direction: column;
}
.item__date {
  color: #b5abde;
  font-size: 10px;
}

::-webkit-scrollbar {
  width: 7px;
  height: 8px;
  background-color: #f1edff;
  border-radius: 9em;
}

::-webkit-scrollbar-thumb {
  background-color: rgba(128, 128, 128, 0.777);
  border-radius: 9em;
  box-shadow: inset 1px 1px 10px #f3faf7;
}

::-webkit-scrollbar-button:vertical:start:decrement,
::-webkit-scrollbar-button:vertical:end:increment,
::-webkit-scrollbar-button:horizontal:start:decrement,
::-webkit-scrollbar-button:horizontal:end:increment {
  display: none;
}
</style>