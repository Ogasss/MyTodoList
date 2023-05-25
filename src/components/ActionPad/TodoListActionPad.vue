<template>
    <transition name="actionPadTransition"> 
        <div @click.self="showActionPad" v-show="padVisible" key="theWrapper" class="theActionPadWrapper">
            <div @click.self="showActionPad" v-show="padVisible"  class="actionPadWrapper">
                <div class="actionPad"> 
                    <button v-for="action in actionList" :key="action.title" @click="actions(action)">
                        {{action}}
                    </button>
                </div>
            </div>
        </div>
    </transition>
</template>

<script>
export default {
    data(){
        return {
            padVisible: false,
            actionList:[
                '查看代办',
                '查看所有',
                '清空所有',
                '清空完成',
                '查看完成',
            ]
        }
    },
    methods:{
        showActionPad(){
            this.padVisible = !this.padVisible
        },
        actions(value){
            this.$eventBus.$emit('actions',value)
        }
    },
    mounted(){
        this.$eventBus.$on('showActionPad',()=>{
            this.showActionPad()
        })
    }
}
</script>

<style scoped>
    .theActionPadWrapper{
        position: fixed;
        /* background: rgba(0, 0, 0, 0.1); */
        top: 0;
        left: 0;
        width: 100vw;
        height: 100vh;
    }
    .actionPad{
        border-left: 1px solid rgb(220, 220, 220);
        position: absolute;
        right: 0;
        top: 0;
        height: 100%;
        width: 50%;
        padding: 0.25rem 0 0 0.25rem;
        background: white;
        display: flex;
        flex-direction: column;
        align-items: center;
    }
        .actionPad button{
            border: 1px solid rgb(220, 220, 220);
            padding: 0.25rem;
            width: 2.5rem;
            font-size: 0.3rem;
            margin-right: 0.25rem;
            margin-bottom: 0.5rem;
            transition: all ease 0.3s;
        }
        .actionPad button:hover{
            border: 1px solid rgb(0, 205, 205);
            background: rgb(0, 205, 205);
            color: white;
        }

.actionPadTransition-enter{
    transform: translateX(6rem);
}
.actionPadTransition-enter-active{
    transition: all ease 0.3s;
}
.actionPadTransition-enter-to{
    opacity: 1;
    transform: translateX(0);
}

.actionPadTransition-leave{
    opacity: 1;
    transform: translateX(0);
}
.actionPadTransition-leave-active{
    transition: all ease 0.3s;
}
.actionPadTransition-leave-to{
    opacity: 0;
    transform: translateX(6rem);
}

</style>