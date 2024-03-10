<script lang="ts">
import OpenAI from "openai";
import MarkdownIt from "markdown-it";
import { ElNotification } from "element-plus";

import "element-plus/theme-chalk/src/base.scss";
import "element-plus/theme-chalk/src/notification.scss";

export default {
  data() {
    return {
      modelList: [
        "请选择模型",
        "gpt-3.5-turbo",
        "gpt-3.5-turbo-0301",
        "gpt-4",
        "text-embedding-ada-002",
        "moonshot-v1-8k"
      ],
      selectedModel: "请选择模型",
      responseContent: "",
    };
  },
  methods: {
    async main() {
      const password = prompt("请输入密码");
      if (password === import.meta.env.VITE_PASSWORD) {
        const openai = new OpenAI({
          baseURL: import.meta.env.VITE_BASE_URL || import.meta.env.VITE_KIMI_URL,
          apiKey: import.meta.env.VITE_OPENAI_API_KEY || import.meta.env.VITE_KIMI_KEY,
          dangerouslyAllowBrowser: true,
        });

        const Warning = () => {
          ElNotification({
            title: "无效",
            message: "你选择的模型无效，请重新选择",
            type: "warning",
          });
        };

        if (this.selectedModel === this.modelList[0]) {
          Warning();
          return;
        }

        const textInput = (
          document.getElementById("textInput") as HTMLInputElement
        ).value;

        const chatCompletion = await openai.chat.completions.create({
          messages: [{ role: "user", content: textInput }],
          model: this.selectedModel,
        });

        const messageContent = chatCompletion.choices[0]?.message.content;
        console.log(messageContent);
        if (messageContent !== null && messageContent !== undefined) {
          this.responseContent = this.parseMarkdown(messageContent);
        }
        
      } else {
        alert("密码错误");
      }
      
    },
    
    parseMarkdown(content: string) {
      const md = new MarkdownIt();
      return md.render(content);
    },

    saveTextAsFile(text: string, filename: string) {
      const blob = new Blob([text], { type: "text/plain" });
      const url = URL.createObjectURL(blob);
      const link = document.createElement("a");
      link.href = url;
      link.download = filename;
      link.click();
      URL.revokeObjectURL(url);
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
      <option v-for="model in modelList" :key="model" :value="model">
        {{ model }}
      </option>
    </select>
  </div>

  <div class="content">
    <p style="text-align: center">此处为回答</p>
    <div v-html="responseContent" id="response"></div>
  </div>
  <div class="textdev">
    <textarea name="" id="textInput" cols="30" rows="10"></textarea>
  </div>
  <button @click="main()" class="btn">开始回答</button><button @click="saveTextAsFile(responseContent, 'response.txt')" class="btn">保存文本</button>
</template>
