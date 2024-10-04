<script setup>
import { editLogs } from './utils';
import { ref } from 'vue';

const modifiedContent = ref(null);

const handleFileUpload = (event) => {
  const file = event.target.files[0];
  if (file) {
    const reader = new FileReader();
    reader.onload = (e) => {
      const content = e.target.result;
      modifyContent(content);
    };
    reader.readAsText(file);
  }
};

const modifyContent = (content) => {
  // Здесь можно изменить содержимое файла
  // Например, добавим строку "Изменено:" к началу файла
  modifiedContent.value = editLogs(content)
};

const downloadFile = () => {
  const blob = new Blob([modifiedContent.value], { type: 'text/plain' });
  const url = URL.createObjectURL(blob);
  const link = document.createElement('a');
  link.href = url;
  link.download = 'output.log'; // Имя нового файла
  document.body.appendChild(link);
  link.click();
  document.body.removeChild(link);
  URL.revokeObjectURL(url); // Освобождаем память
  modifiedContent.value = ''
};
</script>

<template>
  <div>
    <a href="https://forum.minefite.net/topic/6163-%D0%BA%D1%83%D0%BB-%D1%82%D0%B8%D1%80%D0%B0%D1%81%D0%BC%D0%BE%D1%80%D0%B5-%D0%BD%D0%B0%D1%88-%D0%B4%D0%BE%D0%BC" target="_blank">
      <img src="/Kul_Tiran_crest.webp" class="logo" alt="Kul_Tiran_crest logo" />
    </a>
    <h2>ФОРМАТИРОВАНИЕ ЛОГОВ</h2>
    <div>
      <label for="file-upload" class="custom-file-upload">
        Загрузить файл .log
      </label>
      <input @change="handleFileUpload" id="file-upload" type="file" />
    </div>
    <button  v-if="modifiedContent" @click="downloadFile">Скачать модифицированный файл</button>
    <p>СОЗДАНО ПРИ ПОДДЕРЖКЕ ФРАКЦИИ КУЛ-ТИРАС</p>
    <a
      href="https://forum.minefite.net/profile/10996-elot/">Автор: Elot(Максим)</a>
  </div>
</template>

<style scoped>
.logo {
  height: 10em;
  padding: 1.5em;
  will-change: filter;
  transition: filter 300ms;
}
.logo:hover {
  filter: drop-shadow(0 0 2em #646cffaa);
}
.logo.vue:hover {
  filter: drop-shadow(0 0 2em #42b883aa);
}

input[type="file"] {
  display: none;
}

.custom-file-upload {
  border: 1px solid #ccc;
  display: inline-block;
  padding: 6px 12px;
  cursor: pointer;
}
</style>
