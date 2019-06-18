<template>
    <div class="tree-wrapper">
        <v-node v-for="item in 5" :key="item">{{item}}</v-node>
    </div>
</template>

<script>
import Node from '@/components/Node'

const DRAG_DOM_CLASS = 'drag-node'
const NODE_HEIGHT = 28

export default {
    data () {
        return {
            $dragTargetDom: null,
            $placeholderNode: null
        }
    },
    mounted() {
        this.$el.addEventListener('mousedown', this.mouseDown)
    },
    methods: {
        mouseDown (e) {
            if (!e.target.classList.contains(DRAG_DOM_CLASS)) {
                return
            }
            this.$dragTargetDom = e.target
            this.insertPlacehodlerDom()
            this.$dragTargetDom.classList.add('node__drag-target')
            this.touchPointX = e.clientX - this.$dragTargetDom.offsetLeft
            this.touchPointY = e.clientY - this.$dragTargetDom.offsetTop
            document.addEventListener('mousemove', this.mouseMove)
            document.addEventListener('mouseup', this.mouseUp)
        },
        mouseMove (e) {
            const top = e.clientY - this.touchPointY
            const left = e.clientX - this.touchPointX

            const index = Math.round((top - this.$el.offsetTop) / NODE_HEIGHT + 1)
            this.oldIndex !== index && this.$el.insertBefore(this.$placeholderNode, this.$el.childNodes[index])
            this.oldIndex = index

            this.$dragTargetDom.style.top = top + 'px'
            this.$dragTargetDom.style.left = left + 'px'
        },
        mouseUp () {
            document.removeEventListener('mousemove', this.mouseMove)
            document.removeEventListener('mouseup', this.mouseUp)
            this.$dragTargetDom.removeAttribute('style')
            this.$dragTargetDom.classList.remove('node__drag-target')
            this.$el.insertBefore(this.$dragTargetDom, this.$placeholderNode)
            this.$placeholderNode.remove()
        },
        insertPlacehodlerDom () {
            this.$placeholderNode = this.$dragTargetDom.cloneNode(true)
            this.$placeholderNode.classList.add('node__placeholder')
            this.$el.insertBefore(this.$placeholderNode, this.$dragTargetDom.nextElementSibling)
        }
    },
    components: {
        vNode: Node
    }
}
</script>

<style lang="less" scoped>
.tree-wrapper {
    position: relative;
    .node__placeholder {
        opacity: .5;
    }
    .node__drag-target {
        position: fixed;
    }
}
</style>
