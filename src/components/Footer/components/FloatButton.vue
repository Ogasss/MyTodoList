<template>
    <div @click.self="showActionPad" class="theFloatButtonWrapper">
        {{buttonWord}}
    </div>
</template>

<script>
export default {
    data(){
        return{
            buttonWord: '操作',
        }
    },
    methods:{
        showActionPad(){
            this.$eventBus.$emit('showActionPad')
        }
    }
}
</script>

<style>
    .theFloatButtonWrapper{
        position: fixed;
        right: 0.5rem;
        bottom: 0.5rem;
        width: 1.2rem;
        height: 1.2rem;
        border-radius: 2rem;
        background: rgb(0, 205, 205);
        color: white;
        display: flex;
        align-items: center;
        justify-content: center;
        font-size: 0.35rem;
    }
</style>