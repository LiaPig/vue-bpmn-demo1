# vue-bpmn-demo

> vue-bpmn

## Build Setup

``` bash
# install dependencies
npm install

# serve with hot reload at localhost:8080
npm run dev

# build for production with minification
npm run build

# build for production and view the bundle analyzer report
npm run build --report
```

For a detailed explanation on how things work, check out the [guide](http://vuejs-templates.github.io/webpack/) and [docs for vue-loader](http://vuejs.github.io/vue-loader).

---
由于之前的公司的项目中的工作流管理要用到流程图，而``bpmn-js``官方的文档是全英的而且使用的``js``框架是``jQuery``，可能是比较新的技术，官方也还在不断的更新，相关的文档或者资料很少很难找，只好自己不断爬坑填坑了。

# 什么是bpmn-js？
一个``BPMN 2.0``渲染工具包和Web建模器。
github地址：https://github.com/bpmn-io
实例地址：[https://bpmn.io/toolkit/bpmn-js/](https://bpmn.io/toolkit/bpmn-js/)

## 1.先从简单开始，能获取服务器上的流程图并显示出来：
安装相关的依赖都是必须的，可以在官方文档上查看，在这里就不详细讲了。
+ html: (界面很简单，这些都是必需的。)
```html
<template>
  <div class="containers" ref="content">
    <div class="canvas" ref="canvas"></div>
    <div id="js-properties-panel" class="panel"></div>
  </div>
</template>
```
+ js:

```javscript
<script>
  // 引入相关的依赖
  import BpmnViewer from 'bpmn-js'
  import BpmnModeler from 'bpmn-js/lib/Modeler'
  import propertiesPanelModule from 'bpmn-js-properties-panel'
  import propertiesProviderModule from 'bpmn-js-properties-panel/lib/provider/camunda'
  import camundaModdleDescriptor from 'camunda-bpmn-moddle/resources/camunda'


  export default {
    data(){
      return {
        // bpmn建模器
        bpmnModeler: null,
        container: null,
        canvas: null
      }
    },
    methods:{
      createNewDiagram() {
        const bpmnXmlStr = '<?xml version="1.0" encoding="UTF-8"?>\n' +
          '<bpmn:definitions xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:bpmn="http://www.omg.org/spec/BPMN/20100524/MODEL" xmlns:bpmndi="http://www.omg.org/spec/BPMN/20100524/DI" xmlns:dc="http://www.omg.org/spec/DD/20100524/DC" xmlns:di="http://www.omg.org/spec/DD/20100524/DI" id="Definitions_0fppxr8" targetNamespace="http://bpmn.io/schema/bpmn">\n' +
          '  <bpmn:process id="Process_1" isExecutable="false">\n' +
          '    <bpmn:startEvent id="StartEvent_1" name="begin&#10;">\n' +
          '      <bpmn:outgoing>SequenceFlow_0nrfbee</bpmn:outgoing>\n' +
          '    </bpmn:startEvent>\n' +
          '    <bpmn:task id="Task_0ho18x0" name="hello&#10;">\n' +
          '      <bpmn:incoming>SequenceFlow_0nrfbee</bpmn:incoming>\n' +
          '      <bpmn:outgoing>SequenceFlow_00ho26x</bpmn:outgoing>\n' +
          '    </bpmn:task>\n' +
          '    <bpmn:task id="Task_1ymuvem" name="world">\n' +
          '      <bpmn:incoming>SequenceFlow_00ho26x</bpmn:incoming>\n' +
          '      <bpmn:outgoing>SequenceFlow_18df8vb</bpmn:outgoing>\n' +
          '    </bpmn:task>\n' +
          '    <bpmn:endEvent id="EndEvent_1c0ed2n" name="end">\n' +
          '      <bpmn:incoming>SequenceFlow_18df8vb</bpmn:incoming>\n' +
          '    </bpmn:endEvent>\n' +
          '    <bpmn:sequenceFlow id="SequenceFlow_0nrfbee" sourceRef="StartEvent_1" targetRef="Task_0ho18x0" />\n' +
          '    <bpmn:sequenceFlow id="SequenceFlow_00ho26x" sourceRef="Task_0ho18x0" targetRef="Task_1ymuvem" />\n' +
          '    <bpmn:sequenceFlow id="SequenceFlow_18df8vb" sourceRef="Task_1ymuvem" targetRef="EndEvent_1c0ed2n" />\n' +
          '  </bpmn:process>\n' +
          '  <bpmndi:BPMNDiagram id="BPMNDiagram_1">\n' +
          '    <bpmndi:BPMNPlane id="BPMNPlane_1" bpmnElement="Process_1">\n' +
          '      <bpmndi:BPMNShape id="_BPMNShape_StartEvent_2" bpmnElement="StartEvent_1">\n' +
          '        <dc:Bounds x="173" y="102" width="36" height="36" />\n' +
          '        <bpmndi:BPMNLabel>\n' +
          '          <dc:Bounds x="178" y="145" width="27" height="27" />\n' +
          '        </bpmndi:BPMNLabel>\n' +
          '      </bpmndi:BPMNShape>\n' +
          '      <bpmndi:BPMNShape id="Task_0ho18x0_di" bpmnElement="Task_0ho18x0">\n' +
          '        <dc:Bounds x="485" y="244" width="100" height="80" />\n' +
          '      </bpmndi:BPMNShape>\n' +
          '      <bpmndi:BPMNShape id="Task_1ymuvem_di" bpmnElement="Task_1ymuvem">\n' +
          '        <dc:Bounds x="712" y="391" width="100" height="80" />\n' +
          '      </bpmndi:BPMNShape>\n' +
          '      <bpmndi:BPMNShape id="EndEvent_1c0ed2n_di" bpmnElement="EndEvent_1c0ed2n">\n' +
          '        <dc:Bounds x="1056" y="568" width="36" height="36" />\n' +
          '        <bpmndi:BPMNLabel>\n' +
          '          <dc:Bounds x="1065" y="611" width="19" height="14" />\n' +
          '        </bpmndi:BPMNLabel>\n' +
          '      </bpmndi:BPMNShape>\n' +
          '      <bpmndi:BPMNEdge id="SequenceFlow_0nrfbee_di" bpmnElement="SequenceFlow_0nrfbee">\n' +
          '        <di:waypoint x="209" y="120" />\n' +
          '        <di:waypoint x="347" y="120" />\n' +
          '        <di:waypoint x="347" y="284" />\n' +
          '        <di:waypoint x="485" y="284" />\n' +
          '      </bpmndi:BPMNEdge>\n' +
          '      <bpmndi:BPMNEdge id="SequenceFlow_00ho26x_di" bpmnElement="SequenceFlow_00ho26x">\n' +
          '        <di:waypoint x="585" y="284" />\n' +
          '        <di:waypoint x="649" y="284" />\n' +
          '        <di:waypoint x="649" y="431" />\n' +
          '        <di:waypoint x="712" y="431" />\n' +
          '      </bpmndi:BPMNEdge>\n' +
          '      <bpmndi:BPMNEdge id="SequenceFlow_18df8vb_di" bpmnElement="SequenceFlow_18df8vb">\n' +
          '        <di:waypoint x="812" y="431" />\n' +
          '        <di:waypoint x="934" y="431" />\n' +
          '        <di:waypoint x="934" y="586" />\n' +
          '        <di:waypoint x="1056" y="586" />\n' +
          '      </bpmndi:BPMNEdge>\n' +
          '    </bpmndi:BPMNPlane>\n' +
          '  </bpmndi:BPMNDiagram>\n' +
          '</bpmn:definitions>\n'
        // 将字符串转换成图显示出来
        this.bpmnModeler.importXML(bpmnXmlStr, function (err) {
          if (err) {
            console.error(err);
          }
          else {
            // 这里还没用到这个，先注释掉吧
            // that.success()
          }
        })
      }
    },
    mounted(){
      // 获取到属性ref为“content”的dom节点
      this.container = this.$refs.content
      // 获取到属性ref为“canvas”的dom节点
      const canvas = this.$refs.canvas

      // 建模，官方文档这里讲的很详细
      this.bpmnModeler = new BpmnModeler({
        container: canvas,
        //添加控制板
        propertiesPanel: {
          parent: '#js-properties-panel'
        },
        additionalModules: [
          // 左边工具栏以及节点
          propertiesProviderModule,
          // 右边的工具栏
          propertiesPanelModule
        ],
        moddleExtensions: {
          camunda: camundaModdleDescriptor
        }
      });
      this.createNewDiagram(this.bpmnModeler);
    }
  }
</script>
```
+ css: (记得引入样式，否则左边的工具栏无法显示T_T 别问我怎么知道的)
```
<style lang="scss">
  /*左边工具栏以及编辑节点的样式*/
  @import 'bpmn-js/dist/assets/diagram-js.css';
  @import 'bpmn-js/dist/assets/bpmn-font/css/bpmn.css';
  @import 'bpmn-js/dist/assets/bpmn-font/css/bpmn-codes.css';
  @import 'bpmn-js/dist/assets/bpmn-font/css/bpmn-embedded.css';
  /*右边工具栏样式*/
  @import 'bpmn-js-properties-panel/dist/assets/bpmn-js-properties-panel.css';
  .containers{
    position: absolute;
    background-color: #ffffff;
    width: 100%;
    height: 100%;
  }
  .canvas{
    width: 100%;
    height: 100%;
  }
  .panel{
    position: absolute;
    right: 0;
    top: 0;
    width: 300px;
  }
</style>

```

![有图有真相](https://upload-images.jianshu.io/upload_images/7016617-fc4e4996881aaba1.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
