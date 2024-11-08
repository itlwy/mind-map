<template>
    <div @click="onItemClick"  class="order-node-content">
        <p class="node-text">
            {{ node.nodeData.data.text }}
            <span
                v-if="node.nodeData.data.orderIndex !== 0 && node.nodeData.data.orderIndex !== null && node.nodeData.data.orderIndex !== undefined" class="order-index">
                ({{ node.nodeData.data.orderIndex }})
            </span>
        </p>
    </div>
</template>

<script>
export default {
    name: 'OrderNodeContent',
    props: {
        node: {
            type: Object,
            required: true
        },
        onCallback: {
            type: Function,
            required: false
        }
    },
    mount() {

    },
    methods: {
        onItemClick() {
            if (typeof this.onCallback === 'function') {
                this.onCallback(this.node);
            }
        },
    }
}
</script>

<style>
.order-node-content {  
    padding: 10px; /* 根据需要调整padding大小 */  
    cursor: pointer; /* 添加点击指示 */  
}  
  
.node-text {  
    color: white; /* 设置字体颜色为白色 */  
}  
  
.order-index {  
    color: white; /* 确保索引号也是白色 */  
    margin-left: 5px; /* 给索引号添加一些左边距 */  
}  
</style>