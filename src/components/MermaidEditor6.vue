
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
import { ArrowDownOnSquareIcon  } from "@heroicons/vue/24/outline";

//file-type-svg
import { IconFileTypeSvg } from "@tabler/icons-vue";


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
    //icon: ArrowDownOnSquareIcon,
    icon: IconFileTypeSvg,
    name: "save",
    tooltip: "save svg",
  },
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

const code_old = ref(`---
title: Hello Title
config:
  theme: forest
---
flowchart
	Hello --> World
`)

const code = ref(`---
title: Hello Title
config:
  theme: forest
---


flowchart LR
    subgraph subgraph1
        direction TB
        top1[top] --> bottom1[bottom]
    end
    subgraph subgraph2
        direction TB
        top2[top] --> bottom2[bottom]
    end
    %% ^ These subgraphs are identical, except for the links to them:

    %% Link *to* subgraph1: subgraph1 direction is maintained
    outside --> subgraph1
    %% Link *within* subgraph2:
    %% subgraph2 inherits the direction of the top-level graph (LR)
    outside ---> top2
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


async function saveFile(filename, content) {
 try {
    // showSaveFilePickerがブラウザでサポートされているかどうかを確認する
if ('showSaveFilePicker' in window) {
        // ユーザーがファイルにアクセスできるかどうかを確認する
if (navigator.userActivation.isActive) {
   	//const filename = 'test.svg';
    	//const content = 'Hello World!';
            const options = await getOptions(filename, 'svg');
            const handle = await window.showSaveFilePicker(options);
            const writable = await handle.createWritable();
            await writable.write(content);
    				await writable.close();
    				console.log('ファイルが正常に保存されました');
    			} else {
            throw new Error('navigator.userActivationが有効状態になっていません');
        }
    } else {
        throw new Error('showSaveFilePickerがサポートされていません');
    }
 } catch (error) {
    		// 保存失敗
    console.error(error);
    alert(error);
  }
}

 // オプション取得
function getOptions(filename, type) {
 switch (type) {
    case 'text': return { suggestedName: filename, types: [{ accept: { 'text/plain': ['.txt'] } }] }
    		default: break;
    	}
return { suggestedName: filename };
}

function prettifyXml(sourceXml)
{
    var xmlDoc = new DOMParser().parseFromString(sourceXml, 'application/xml');
    var xsltDoc = new DOMParser().parseFromString([
        // describes how we want to modify the XML - indent everything
        '<xsl:stylesheet xmlns:xsl="http://www.w3.org/1999/XSL/Transform">',
        '  <xsl:strip-space elements="*"/>',
        '  <xsl:template match="para[content-style][not(text())]">', // change to just text() to strip space in text nodes
        '    <xsl:value-of select="normalize-space(.)"/>',
        '  </xsl:template>',
        '  <xsl:template match="node()|@*">',
        '    <xsl:copy><xsl:apply-templates select="node()|@*"/></xsl:copy>',
        '  </xsl:template>',
        '  <xsl:output indent="yes"/>',
        '</xsl:stylesheet>',
    ].join('\n'), 'application/xml');

    var xsltProcessor = new XSLTProcessor();
    xsltProcessor.importStylesheet(xsltDoc);
    var resultDoc = xsltProcessor.transformToDocument(xmlDoc);
    var resultXml = new XMLSerializer().serializeToString(resultDoc);
    return resultXml;
};

async function toolbarItemClick(data) {
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

  } else if (data == "save") {
     let filename = "test.svg";

     let render = document.getElementById("mermaid_render");
     console.log(render);
     let svgNode = render.querySelector("svg")

     const svgXml = new XMLSerializer().serializeToString(svgNode);

     let svgText = prettifyXml(svgXml);

     saveFile(filename, svgText);
     //let content = "hello world!!!";
     //saveFile(filename, content);
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
	    <div  id="mermaid_render">
       <VueMermaidRender   :config="config" :content="code" @err-mermaid="err_mermaid"/>
	    </div>
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

