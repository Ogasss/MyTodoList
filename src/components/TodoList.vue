<template>
    <div class="theWrapper">
        <TodoListHeader class="theHeader"/>
        <TodoListMain class="theMain"/>
        <TodoListFooter class="theFooter"/>
        <TodoListActionPad class="theActionPad"/>
    </div>
</template>

<script>
import TodoListHeader from './Header/TodoListHeader.vue'
import TodoListMain from './Main/TodoListMain'
import TodoListFooter from './Footer/TodoListFooter'
import TodoListActionPad from './ActionPad/TodoListActionPad.vue'

export default {
    components: {
        TodoListHeader,
        TodoListMain,
        TodoListFooter,
        TodoListActionPad
    },
}
</script>

<style scoped>
    .theWrapper{
        width: 100%;
        height: 100%;
    }
    .theHeader{
        position: fixed;
        top: 0;
        background: white;
        z-index: 1;
    }
    .theMain{
        margin-top: 2.5rem;
        width: 100%;
    }
    .theFooter{

    }
    .theActionPad{
        z-index: 1;
    }
</style>