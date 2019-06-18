<template>
    <div class="node-wrapper drag-node">
        This is Node Wrapper <slot></slot>
    </div>
</template>

<script>
export default {
    name: 'tree-node'
}
</script>

<style lang="less" scoped>
.node-wrapper {
    cursor: pointer;
    user-select: none;
    line-height: 28px;
}
</style>
