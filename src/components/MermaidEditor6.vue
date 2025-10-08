
<script setup lang="ts">

import { ref, computed , onMounted } from 'vue';
//import { VueMermaidRender } from 'vue-mermaid-render';
import  VueMermaidRender  from './VueMermaidRender.vue';

import { CodeEditor } from 'monaco-editor-vue3';
import * as monaco from 'monaco-editor';

// Import the CSS styles
import 'monaco-editor-vue3/dist/style.css';


import mermaid_initEditor from 'monaco-mermaid';


import { Tabs, Tab } from 'super-vue3-tabs'

import VueSplitter from '@rmp135/vue-splitter'

import BaseDemo from './BaseDemo.vue'
import { GenLorum } from './Helpers'
import  Ref  from './Ref.vue'


import { onErrorCaptured } from "vue";

import Toolbar from "./toolbar/Toolbar.vue";
import Select from "./toolbar/Select.vue";

import { ArrowUturnRightIcon } from "@heroicons/vue/24/outline";
import { ArrowUturnLeftIcon } from "@heroicons/vue/24/outline";

import { Cog6ToothIcon } from "@heroicons/vue/24/outline";
import { EllipsisVerticalIcon } from "@heroicons/vue/24/outline";
import { EllipsisHorizontalIcon } from "@heroicons/vue/24/outline";
//magnifying-glass-plus
//magnifying-glass-minus
import { MagnifyingGlassPlusIcon  } from "@heroicons/vue/24/outline";
import { MagnifyingGlassMinusIcon  } from "@heroicons/vue/24/outline";
import { BookOpenIcon  } from "@heroicons/vue/24/outline";


const doc_pane_percent = ref(100);

monaco.editor.defineTheme('vs2', {
    base: 'vs-dark',
    inherit: true,
    rules: [
      {
        token: "identifier",
        foreground: "#9CDCFE"
      },
      {
        token: "identifier.function",
        foreground: "#DCDCAA"
      },
      {
        token: "type",
        foreground: "#1AAFB0"
      },
    ],
    colors: {}
    });

const toolbar_define = [
  {
    icon: ArrowUturnLeftIcon,
    name: "undo",
    tooltip: "undo",
  },
  {
    icon: ArrowUturnRightIcon,
    name: "redo",
    tooltip: "redo",
  },
  {
    icon: MagnifyingGlassPlusIcon ,
    name: "zoomin",
    tooltip: "Zoom In",
    handler: "clickB",
  },
  {
    icon: MagnifyingGlassMinusIcon ,
    name: "zoomout",
    tooltip: "Zoom Out",
    handler: "clickB",
  },

  {
    icon: BookOpenIcon ,
    name: "book",
    toggle: true,
    tooltip: "Book Open",
    handler: "clickB",
    alignright: true,
  },
  {
    icon: Select,
    select: true,
    isClearable: false,
    placeholder: "dark",
    name: "theme",
    options: [
      { label: "light", value: "mermaid" },
      { label: "dark" , value: "mermaid-dark" },
    ],

    tooltip: "Select         OK",
  },

   {
    icon: EllipsisVerticalIcon,
    name: "EllipsisVerticalIcon",
    tooltip: "B         OK",
  },
  { icon: Cog6ToothIcon, name: "Cog6ToothIcon", tooltip: "B         OK" },

];


onErrorCaptured((error, insttance, info) => {
  // error: エラーのインスタンス
  // instance: ソースコンポーネント
  // info: なんかの情報

  // errorの情報をもとにハンドリング
  console.log("エラーが発生しました。")
});

const lorumipsum = GenLorum()

const config_json = ref(`
{ 
  "theme" : "forest",
  "startOnLoad" : false, 
  "flowchart" : { "useMaxWidth" : false, "htmlLabels" : true }
}

`);

/*
const config = ref(
{ 
  //theme : "dark",
  theme : "forest",
  startOnLoad: true, 
  flowchart: { useMaxWidth: false, htmlLabels: true }
 }
);
*/

const config = computed(() => {
  return  JSON.parse(config_json.value);
})


//%%{init:{'theme':'forest'}}%%
const code_ = ref(`
sequenceDiagram
        Site->>mermaid: initialize
        Site->>mermaid: content loaded
        mermaid->>mermaidAPI: init
`)


const code2 = ref(`
%%{
 init: { 'layout':'elk',
          'theme': 'forest' 
       } 
}%%

flowchart TB
  A[Start] --> B{Decision}
  B -->|Yes| C[Continue]
  B -->|No| D[Stop]



`)

const code = ref(`---
title: Hello Title
config:
  theme: forest
---
flowchart
	Hello --> World
`)

let  editorOptions = {
  fontSize: 14,
  minimap: { enabled: false },
  automaticLayout: true
};


function handleError(error) {
 console.log("Error:", error);
}
function handleLoading() {
 console.log("Loading:" );
}
function handleReady() {
 console.log("Redy:" );
}
function err_mermaid(msg) {
 console.log("Err Mermaid:", msg );
}

const code_editor = ref();
let editorInstance: monaco.editor.IStandaloneCodeEditor | null = null;
mermaid_initEditor(monaco); 

//monaco.editor.defineTheme('vs2', theme)
const onEditorMount = (editor: monaco.editor.IStandaloneCodeEditor) => {
  editorInstance = editor;
//initEditor(monaco); 
  //monaco.editor.setTheme('vs2');
  //monaco.editor.setTheme('mermaid');
  monaco.editor.setTheme('mermaid-dark');
  //monaco.editor.setTheme('vs');
  //monaco.editor.setTheme('vs-white');
  //monaco.editor.setTheme('vs-dark');
  //monaco.editor.setTheme('hc-black');

  //console.log("option",editorInstance.getOption(monaco.editor.EditorOption.fontSize));

  //monaco.editor.defineTheme('myTheme', theme)
  /*
  monaco.editor.defineTheme('myTheme', {
        base: 'vs',
        inherit: true,
        rules: [],
        colors: {
            'editor.background': '#efefef',
        },
    });
    */
};

  monaco.editor.defineTheme('myTheme', {
        base: 'vs',
        inherit: true,
        rules: [],
        colors: {
            'editor.background': '#efefef',
        },
    });


function incFontSize  () {
  if (editorInstance) {
    const currentSize = editorInstance.getOption(monaco.editor.EditorOption.fontSize);
    editorInstance.updateOptions({ fontSize: currentSize + 1 });
  }
};
function decFontSize  ()  {
  if (editorInstance) {
    const currentSize = editorInstance.getOption(monaco.editor.EditorOption.fontSize);
    editorInstance.updateOptions({ fontSize: currentSize - 1 });
  }
};
function toolbarItemClick(data) {
  console.log(" App toolbar click:", data);

  if (data == "zoomin" ) {
      incFontSize();
  } else if (data == "zoomout") {
      decFontSize();

  } else if (data == "undo") {
     const model = editorInstance.getModel();
     //model.canUndo();
     model.undo();

  } else if (data == "redo") {
     const model = editorInstance.getModel();
     //model.canRedo();
     model.redo();

  }

}
function toolbarItemToggle(name, state) {
  console.log(" App toolbar toggle:", name, state);
  if ( name == "book" ) {
      if (doc_pane_percent.value == 100 ) {
          doc_pane_percent.value = 50;
      } else {
          doc_pane_percent.value = 100;
      }
      // console.log(doc_pane_percent.value);

  }
}

function toolbarItemSelect(type, theme_name) {
  console.log(" App toolbar select:", type, theme_name);
  if ( type == "theme" ) {
     monaco.editor.setTheme(theme_name);
  }
}

function toolbarItemSelectColor(name, data) {
  console.log(" App toolbar select color:", name, data);
}

function toolbarItemRadio(radio_name, radio_index, name, state) {
  console.log(" App toolbar Radio:", radio_name, radio_index, state);
}




</script>



<template>


  <base-demo  initial-Percent=40
  >
    <template #left-pane>
    
       <VueMermaidRender :config="config" :content="code" @err-mermaid="err_mermaid"/>
	   
    </template>
    <template #right-pane>
    <Toolbar
      :toolbar_define="toolbar_define"
      :icon_flat_mode="false"
      @toolbarItemClick="toolbarItemClick"
      @toolbarItemToggle="toolbarItemToggle"
      @toolbarItemSelect="toolbarItemSelect"
      @toolbarItemRadio="toolbarItemRadio"
      class="toolbar"
    />

      <vue-splitter
        class="container"
        is-horizontal
	:initial-percent="100"
	v-model:percent="doc_pane_percent"
      >
        <template #top-pane  >
            <Tabs  >
              <Tab value="code"  class="code " style="height:100vh;" >
                       <CodeEditor
                         v-model:value="code"
                         language="mermaid"
                         theme="hc-black"
                         :options="editorOptions"
                         @error="handleError"
			 @ready="handleReady"
                         @loading="handleLoading"
			 @editorDidMount="onEditorMount"
                         ref="code_editor"
                       >

                       </CodeEditor>

              </Tab>
              <Tab value="config" class="config"  style="height:100vh;" >
                       <CodeEditor
                         v-model:value="config_json"
                         language="javascript"
                         theme="vs-white"
                         :options="editorOptions"
                       />
              </Tab>
              <Tab value="setting">
                <p>This is the content of Tab 3</p>
              </Tab>
            </Tabs>
	</template>
        <template #bottom-pane >
<!--
          <div>{{lorumipsum}}</div>
-->
	  <Ref />
        </template>
      </vue-splitter>
    </template>
  </base-demo>
</template>

<style scoped>
.toolbar {
  padding : 4px 4px 2px 2px;
  /* background : lightgray;*/
  /*border-bottom : dotted 1px lightgray;*/
}


</style>
<style>
.vue-splitter .splitter-pane {
  overflow-y:hidden;
  overflow-x:hidden;
}


</style>

