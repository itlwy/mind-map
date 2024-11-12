<template>
      <div @click="onItemClick" :class="contentClass">
        <p :style="textStyle">
            <span v-if="node.getData('richText')"  v-html="this.node.getData('text')"></span>
            <span v-else>{{ node.getData('text') }}</span>
            <span 
                :style="textStyle"
                v-if="node.nodeData.data.orderIndex !== 0 && node.nodeData.data.orderIndex !== null && node.nodeData.data.orderIndex !== undefined" 
                class="order-index">
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
        },
        customStyle:{
            type: Object,
            required: true
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
    },
    computed: {  
        textStyle() {  
            return {  
                // color: this.customStyle.color,  
                color: '#000',
                fontSize: this.customStyle.fontSize  
            };  
        },
        contentClass() {
            // 根据 richText 的值返回不同的类
            return this.node.getData('richText') ? 'order-node-content-rich' : 'order-node-content';
        }
    },  
}
</script>

<style>
.order-node-content {    
    padding: 10px; /* 根据需要调整padding大小 */    
    color: white;  
    background-color: #f0f0f0; /* 添加背景颜色 */  
    border: 1px solid #ccc; /* 添加边框 */  
    border-radius: 5px; /* 添加圆角边框（可选） */  
}  
.order-node-content-rich {    
    padding: 10px; /* 根据需要调整padding大小 */    
}  

.order-index {  
    color: white; /* 确保索引号也是白色 */  
    margin-left: 5px; /* 给索引号添加一些左边距 */  
}  
</style>