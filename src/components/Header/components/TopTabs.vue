<template>
    <div class="theTopTabsWrapper">
        <span 
            :class="item.active ? 'tabItem tabActive' : 'tabItem'"
            v-for="item,index in tabList" 
            :key="item.title"
            @click="tabClick(index)"
        >{{item.title}}</span>
        <div class="activeLine" :style="activeLineStyle"></div>
    </div>
</template>

<script>
export default {
    data(){
        return {
            tabList:[
                {
                    title: '今天',
                    active: true,
                },
                {
                    title: '明天',
                    active: false
                },
                {
                    title: '昨天',
                    active: false
                }
            ],
            activeLineStyle:{
                left: '0.3rem'
            }
        }
    },
    methods:{
        tabClick(index){
            this.tabList.forEach((item,_index)=>{
                index !== _index 
                ? 
                this.tabList[_index].active = false 
                :
                this.tabList[_index].active = true
            })
            this.activeLineStyle.left = `${0.3+index*1.3}rem`
            this.$eventBus.$emit('getActiveIndex', index)
            this.$eventBus.$emit('switchTab')
        }
    }
}
</script>

<style scoped>
    
    .theTopTabsWrapper{
        position: relative;
        /* border-bottom: 1px solid rgb(240, 240, 240); */
        height: 1rem;
        display: flex;
        align-items: center;
        --tabWith: 1rem;
    }
        .theTopTabsWrapper .tabItem{
            margin-left: 0.3rem;
            height: 100%;
            width: var(--tabWith);
            display: flex;
            align-items: center;
            justify-content: center;
            transition: all ease 0.3s;
        }

        .theTopTabsWrapper .tabActive{
            font-weight: bold;
        }

    .activeLine{
        position: absolute;
        bottom: 0;
        left: 0.3rem;
        border-bottom: 3px solid rgb(0,205,205);
        width: var(--tabWith);
        transition: all ease 0.3s;
    }
</style>