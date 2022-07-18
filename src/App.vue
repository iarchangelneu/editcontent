<template>
  <div>
    <input type="text" v-model="text" placeholder="Отправка(Enter)" v-on:keyup.enter="editContent">

    <content-test @load="loadContent" :content="content"></content-test>
  </div>
</template>
<script>
import axios from "axios";
import contentTest from './components/contentTest.vue';
export default {
  components: { contentTest },
  data() {
    return {
      text: null,
      content: [],
    }
  },
  mounted() {
    this.loadContent()
  },
  methods: {
    async editContent() {
      const response = await fetch(
        `https://iarchangel-default-rtdb.europe-west1.firebasedatabase.app/people/${this.content.id}.json`,
        {
          method: "PUT",
          header: {
            "Content-Type": "application/json; charset=UTF-8'",
          },
          body: JSON.stringify({
            Text: this.text,
          }),
        }
      );
      this.text = "";
    },

    async loadContent() {
      try {
        const { data } = await axios.get(
          "https://iarchangel-default-rtdb.europe-west1.firebasedatabase.app/people.json"
        );
        if (!data) {
          throw new Error("Список людей пуст");
        }
        this.content = Object.keys(data).map((key) => {
          return {
            id: key,
            ...data[key],
          };
        });
      } catch (e) {
        this.alert = {
          type: "danger",
          title: "Ошибка",
          text: e.message,
        };
      }
    },
  },

}
</script>
<style scoped>
.logo {
  height: 6em;
  padding: 1.5em;
  will-change: filter;
}

.logo:hover {
  filter: drop-shadow(0 0 2em #646cffaa);
}

.logo.vue:hover {
  filter: drop-shadow(0 0 2em #42b883aa);
}
</style>
