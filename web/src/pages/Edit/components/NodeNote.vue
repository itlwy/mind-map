<template>
  <!-- <el-dialog class="nodeNoteDialog" :title="$t('nodeNote.title')" :visible.sync="dialogVisible"
    :width="isMobile ? '90%' : '50%'" :top="isMobile ? '20px' : '15vh'"> -->
  <!-- <el-input
      type="textarea"
      :autosize="{ minRows: 3, maxRows: 5 }"
      placeholder="请输入内容"
      v-model="note"
    >
    </el-input> -->
  <!-- <div class="noteEditor" ref="noteEditor" @keyup.stop @keydown.stop></div> -->
  <!-- <div class="tip">换行请使用：Enter+Shift</div> -->
  <!-- <span slot="footer" class="dialog-footer">
      <el-button @click="cancel">{{ $t('dialog.cancel') }}</el-button>
      <el-button type="primary" @click="confirm">{{
        $t('dialog.confirm')
      }}</el-button>
    </span>
  </el-dialog> -->

  <el-dialog title="please input..." :visible.sync="dialogVisible" width="50%">
    <el-input type="textarea" :autosize="{ minRows: 3, maxRows: 5 }" placeholder="请输入内容" v-model="note" @keyup.native.stop @keydown.native.stop>
    </el-input>

    <span slot="footer" class="dialog-footer">
      <el-button @click="dialogVisible = false">{{ $t('dialog.cancel') }}</el-button>
      <el-button type="primary" @click="handleCustomDialogConfirm">{{
        $t('dialog.confirm')
        }}</el-button>
    </span>
  </el-dialog>
</template>

<script>
import Editor from '@toast-ui/editor'
import '@toast-ui/editor/dist/toastui-editor.css' // Editor's Style
import { isMobile } from 'simple-mind-map/src/utils/index'

/**
 * @Author: 王林
 * @Date: 2021-06-24 22:53:54
 * @Desc: 节点备注内容设置
 */
export default {
  name: 'NodeNote',
  data() {
    return {
      dialogVisible: false,
      note: '',
      activeNodes: [],
      editor: null,
      isMobile: isMobile()
    }
  },
  watch: {
    dialogVisible(val, oldVal) {
      if (!val && oldVal) {
        this.$bus.$emit('endTextEdit')
      }
    }
  },
  created() {
    this.$bus.$on('node_active', this.handleNodeActive)
    this.$bus.$on('showNodeNote', this.handleShowNodeNote)
  },
  beforeDestroy() {
    this.$bus.$off('node_active', this.handleNodeActive)
    this.$bus.$off('showNodeNote', this.handleShowNodeNote)
  },
  methods: {
    handleCustomDialogConfirm() {
      this.activeNodes.forEach(node => {
        node.setNote(this.note)
      })
      this.dialogVisible = false;
    },
    handleNodeActive(...args) {
      this.activeNodes = [...args[1]]
      if (this.activeNodes.length > 0) {
        let firstNode = this.activeNodes[0]
        this.note = firstNode.getData('note') || ''
      } else {
        this.note = ''
      }
    },

    handleShowNodeNote() {
      this.dialogVisible = true
      // 使用非富文本形式
      // 弹出输入框，提示用户输入内容
      // let userInput = prompt('请输入备注:', this.note);

      // // 检查用户是否点击了“取消”
      // if (userInput !== null) {
      //   this.note = userInput
      //   this.activeNodes.forEach(node => {
      //     node.setNote(this.note)
      //   })
      // } else {
      //   console.log('用户取消了输入');
      // }

      // this.$bus.$emit('startTextEdit')
      // this.dialogVisible = true
      // this.$nextTick(() => {
      //   this.initEditor()
      // })
    },

    /**
     * @Author: 王林25
     * @Date: 2022-05-09 11:37:05
     * @Desc: 初始化编辑器
     */
    initEditor() {
      if (!this.editor) {
        this.editor = new Editor({
          el: this.$refs.noteEditor,
          height: '500px',
          initialEditType: 'markdown',
          previewStyle: 'vertical'
        })
      }
      this.editor.setMarkdown(this.note)
    },

    /**
     * @Author: 王林
     * @Date: 2021-06-22 22:08:11
     * @Desc: 取消
     */
    cancel() {
      this.dialogVisible = false
    },

    /**
     * @Author: 王林
     * @Date: 2021-06-06 22:28:20
     * @Desc:  确定
     */
    confirm() {
      this.note = this.editor.getMarkdown()
      this.activeNodes.forEach(node => {
        node.setNote(this.note)
      })
      this.cancel()
    }
  }
}
</script>

<style lang="less" scoped>
.nodeNoteDialog {
  .tip {
    margin-top: 5px;
    color: #dcdfe6;
  }
}
</style>
