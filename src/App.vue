<script lang="ts" >
import OpenAI from 'openai';
import setting from './components/setting.vue';

export default {
  data() {
    return {
      responseContent: '',
    };
  },
  methods: {
    async main() {
      const openai = new OpenAI({
        baseURL: `${setting.base_url}`,
        apiKey: `${setting.api_key}`,
        dangerouslyAllowBrowser: true,
      });

      const chatCompletion = await openai.chat.completions.create({
        messages: [{ role: 'user', content: '你好，你是来自哪个模型？' }],
        model: 'gpt-3.5-turbo',
      });

      const messageContent = chatCompletion.choices[0].message?.content;
      if (messageContent !== null && messageContent !== undefined) {
        this.responseContent = messageContent;
      }
    },
  },
};

// main();

</script>

<template>
<div class="hander"><h1>EASY-GPT-VUE</h1></div>

<div class="content">
  <p style="text-align: center;">此处为回答</p>
  {{ responseContent }}
</div>

<button @click="main();" class="btn">开始回答</button>

</template>

<style scoped>
.hander {
  text-align: center;
  margin-top: 100px;
  font-size: 50px;
  color: #eee;
  text-shadow: #888 0 0 10px;
}

.content {
  text-shadow: #888 0 0 20px;
  margin-left: 50px;
  margin-right: 50px;
  color: #eee;
  border: 2.5px solid #eee;
  border-radius: 10px;
  box-shadow: 10px 10px 5px #888;
  /* text-align: center; */
  margin-top: 100px;
  font-size: 30px;
  backdrop-filter: blur(7.5px);
}

.btn {
  margin-top: 100px;
  margin-left: 45%;
  font-size: 30px;
  color: #f00;
  background: #fff;
  border: 5px solid #f00;
  border-radius: 10px;
  box-shadow: 10px 10px 5px #888;
  padding: 10px 20px;
  transition: all 0.5s;
}

.btn:hover {
  background: #f00;
  color: #fff;
  cursor: pointer;
  box-shadow: 10px 10px 5px #888;
  transition: all 0.5s;
}
</style>
