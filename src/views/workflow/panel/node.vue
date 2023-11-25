<template>
  <div
    ref="node"
    :style="nodeContainerStyle"
    @click="clickNode"
    @mouseup="changeNodeSite"
    @contextmenu.prevent="handleContextMenu($event)"
    :class="nodeContainerClass"
  >
    <!-- 最左侧的那条竖线 -->
    <div class="ef-node-left"></div>
    <!-- 节点类型的图标 -->
    <div class="ef-node-left-ico flow-node-drag">
      <i :class="nodeIcoClass"></i>
    </div>
    <!-- 节点名称 -->
    <div class="ef-node-text" :show-overflow-tooltip="true">
      {{ node.name }}
    </div>
    <!-- 节点状态图标 -->
    <div class="ef-node-right-ico">
      <i class="el-icon-circle-check el-node-state-success" v-show="node.state === 'success'"></i>
      <i class="el-icon-circle-close el-node-state-error" v-show="node.state === 'error'"></i>
      <i class="el-icon-warning-outline el-node-state-warning" v-show="node.state === 'warning'"></i>
      <i class="el-icon-loading el-node-state-running" v-show="node.state === 'running'"></i>
    </div>

    <!-- 右击菜单 -->
    <div v-show="showContextMenu" class="context-menu" :style="{ top: contextMenuTop, left: contextMenuLeft }">
      <ul>
        <li @click="handleClick('delete-node')">删除节点</li>
<!--        <li @click="handleClick('copy-node')">复制节点</li>-->
      </ul>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    node: Object,
    activeElement: Object
  },
  data() {
    return {
      showContextMenu: false,
      contextMenuTop: 0,
      contextMenuLeft: 0
    }
  },
  computed: {
    nodeContainerClass() {
      return {
        'ef-node-container': true,
        'ef-node-active': this.activeElement.type == 'node' ? this.activeElement.nodeId === this.node.id : false
      }
    },
    // 节点容器样式
    nodeContainerStyle() {
      return {
        top: this.node.top,
        left: this.node.left
      }
    },
    nodeIcoClass() {
      var nodeIcoClass = {}
      nodeIcoClass[this.node.ico] = true
      // 添加该class可以推拽连线出来，viewOnly 可以控制节点是否运行编辑
      nodeIcoClass['flow-node-drag'] = this.node.viewOnly ? false : true
      return nodeIcoClass
    }
  },
  methods: {
    // 点击节点
    clickNode() {
      this.$emit('clickNode', this.node.id)
    },
    // 鼠标移动后抬起
    changeNodeSite() {
      // 避免抖动
      if (this.node.left == this.$refs.node.style.left && this.node.top == this.$refs.node.style.top) {
        return;
      }
      this.$emit('changeNodeSite', {
        nodeId: this.node.id,
        left: this.$refs.node.style.left,
        top: this.$refs.node.style.top,
      })
    },
    // 显示右击菜单
    handleContextMenu(event) {
      event.preventDefault();
      this.showContextMenu = true;
      this.contextMenuTop = event.clientY + 'px';
      this.contextMenuLeft = event.clientX + 'px';
    },
    // 点击右击菜单选项
    handleClick(option) {
      if (option === 'delete-node') {
        // 删除节点的逻辑
      } else if (option === 'copy-node') {
        // 复制节点的逻辑
      }
      // 隐藏右击菜单
      this.showContextMenu = false;
    }
  }
}
</script>

<style scoped lang="scss">
.context-menu {
  position: fixed;

  //top: 30px;
  //left: 40px;
  z-index: 10;
  background: #fff;
  //border: 1px solid rgba(177, 177, 177, 0.4);
  border-bottom: 0;
  ul {
    list-style: none;
    font-size: 12px;
    padding: 5px 10px;
    li {
      padding: 10px 10px;
      cursor: pointer;
      border-bottom: 1px solid rgba(177, 177, 177, 0.4);
      &:hover {
        color: cornflowerblue;
      }
    }
  }
}
</style>
