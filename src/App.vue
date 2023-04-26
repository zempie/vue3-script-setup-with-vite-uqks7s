<script setup>
import { ref } from 'vue'
import html2canvas from 'html2canvas'

const editorRef = ref('')
const imgSrc = ref()

async function convertExcel() {
  const editorContent = editorRef.value.textContent
  if (editorContent) {
    const divNode = document.getElementById('editor')

    html2canvas(editorRef.value).then((canvas) => {
      const cv = document.getElementsByTagName('canvas')
      console.log(cv)
      if (cv.length) {
        cv.remove()
      }

      document.body.appendChild(canvas)
      imgSrc.value = canvas?.toDataURL('image/jpeg')
    })
  }
}

function reset() {
  imgSrc.value = ''
  editorRef.value = ''
  const canvasList = document.getElementsByTagName('canvas')
  for (let i = 0; i < canvasList.length; i++) {
    canvasList[i].remove()
  }
}
</script>
<template>
  <div id="app">
    <p>엑셀 테이블 삽입 및 원하는 데이터 삽입</p>
    <div id="editor" contenteditable="true" ref="editorRef"></div>
    <button @click="convertExcel">이미지 변환</button>
    <button @click="reset">초기화</button>

    <div>
      이미지 이미지 형식
      <div><img :src="imgSrc" /></div>
      <p>이미지 소스 : {{ imgSrc }}</p>
      <div></div>
      캔버스 형식
    </div>
  </div>
</template>

<style scoped>
div#editor,
#capture {
  padding: 16px 24px;
  border: 1px solid #d6d6d6;
  border-radius: 4px;
}
</style>
