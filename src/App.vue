<script lang="ts" >
import OpenAI from 'openai';
import MarkdownIt from 'markdown-it';
import { ElNotification } from 'element-plus';

import "element-plus/theme-chalk/src/base.scss";
import "element-plus/theme-chalk/src/notification.scss";

export default {
  data() {
    return {
      modelList: ['请选择模型', 'gpt-3.5-turbo', 'gpt-3.5-turbo-0301', 'gpt-4','text-embedding-ada-002'],
      selectedModel: '请选择模型',
      responseContent: '',
    };
  },
  methods: {
    async main() {
      const openai = new OpenAI({
        baseURL: import.meta.env.VITE_BASE_URL,
        apiKey: import.meta.env.VITE_OPENAI_API_KEY,
        dangerouslyAllowBrowser: true,
      });

      const Warning = () => {
        ElNotification({
          title: '无效',
          message: '你选择的模型无效，请重新选择',
          type: 'warning',
        });
      };
      
      

      if (this.selectedModel === this.modelList[0]) {
        Warning();
        return;
      }

      const textInput = (document.getElementById('textInput') as HTMLInputElement).value;

      const chatCompletion = await openai.chat.completions.create({
        messages: [{ role: 'user', content: textInput }],
        model: this.selectedModel,
      });
      console.log(chatCompletion);
      const messageContent = chatCompletion.choices[0]?.message?.content;
      if (messageContent !== null && messageContent !== undefined) {
        this.responseContent = this.parseMarkdown(messageContent);
      }
    },
    parseMarkdown(content: string) {
      const md = new MarkdownIt();
      return md.render(content);
    }
  },
};



// main();

</script>

<template>
<div class="hander"><h1>EASY-GPT-VUE</h1></div>

<div class="container">
    <!-- <label for="modelSelect">选择模型：</label> -->
    <select id="modelSelect" v-model="selectedModel">
        <option v-for="model in modelList" :key="model" :value="model">{{ model }}</option>
    </select>
</div>

<div class="content">
  <p style="text-align: center;">此处为回答</p>
  <div v-html="responseContent" id="response"></div>
</div>
<div class="textdev">
  <textarea name="" id="textInput" cols="30" rows="10"></textarea>
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
  transition: 0.7s;
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

.container {
  text-align: center;
}

.container label{
  padding: 50px;
  text-align: center;
  margin-top: 100px;
  text-shadow: #888 0 0 20px;
  margin-left: 50px;
  margin-right: 50px;
  color: #eee;
  border: 2.5px solid #eee;
  border-radius: 10px;
  box-shadow: 10px 10px 5px #888;
}

.container select {
  background: none;
  text-align: center;
  /* margin-top: 100px; */
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

option {
  background: none;
  color: #eee;
}

.textdev {
  text-align: center;
}

textarea {
  background: none;
  text-align: center;
  margin-top: 100px;
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
</style>
