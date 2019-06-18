<template>
    <div id="app">
        <v-tree class="tree-el"></v-tree>
    </div>
</template>

<script>
import Tree from '@/components/Tree'

export default {
    name: 'app',
    components: {
        vTree: Tree
    }
}
</script>

<style lang="less">
#app {
    .tree-el {
        margin-top: 50px;
    }
}
</style>
