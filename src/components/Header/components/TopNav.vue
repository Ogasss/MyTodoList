<template>
    <div class="theTopNavWrapper">
        {{title}}
    </div>
</template>

<script>
export default {
    data(){
        return {
            title: '代办清单',
        }
    },
}
</script>

<style scoped>
    .theTopNavWrapper{
        height: 1.5rem;
        font-size: 0.6rem;
        color: rgb(30, 30, 30, 0.9);
        /* font-weight: bold; */
        width: 100vw;
        display: flex;
        align-items: center;
        justify-content: space-between;
        padding: 0 0.25rem;
        transition: all ease 0.3s;
    }
        .theTopNavWrapper .moreAction{
            position: relative;
            width: 1rem;
            height: 0.6rem;
            opacity: 0.8;
            border: 1px solid rgb(205, 205, 205);
            z-index: 3;
        }
        .theTopNavWrapper .actionList{
            position:absolute;
            display: flex;
            flex-direction: column;
            align-items: center;
            justify-content: center;
            top: 0.7rem;
            padding: 0.25rem;
            right: 0rem;
            z-index: 2;
        }   
            .actionList .actionButton{
                border: 1px solid rgb(205, 205, 205);
                background: white;
                width: 1rem;
                height: 0.6rem;
                margin-top: 0.25rem;
            }
    
</style>