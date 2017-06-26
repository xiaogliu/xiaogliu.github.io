---
layout: w
title: Vue2中集成富文本编辑器Quill
date: 2017-06-26 17:41:59
tags: 
---

```js
<template>
	<quill-editor v-model="content"
		ref="myQuillEditor"
		:options="editorOption"
		@change="onEditorChange($event)">
	</quill-editor>
</template>
<script>
  import VueQuillEditor from 'vue-quill-editor';
  import { quillEditor } from 'vue-quill-editor';
  import upload from '../../lib/upload';
  import requests from '../../lib/request';

  export default {
    components: {
      quillEditor
    },
    data () {
      return {
        content: '',
        editorOption: {
          placeholder: '在这里输入图文...'
        }
      }
    },
    // 如果需要手动控制数据同步，父组件需要显式地处理changed事件
    methods: {
      onEditorChange({ editor, html, text }) {
        console.log('editor change!', html)
        this.content = html
      },
    // get the current quill instace object.
    computed: {
      editor() {
        return this.$refs.myQuillEditor.quill
      }
    },
    mounted() {
      // you can use current editor object to do something(quill methods)
      // console.log('this is current quill instance object', this.editor)
    }
  }
</script>
```js