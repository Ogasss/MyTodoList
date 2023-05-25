<template>
    <div class="theMainWrapper">
        <label class="addInputWrapper">
            <input 
            class="addInput"
            @keydown.enter="addItem" type="text" placeholder="输入新的代办" v-model="inputContent"> 
        </label>
        <div class="msgWrapper">
            <div class="numberWrapper">
                已完成：{{completeNumber}}/{{this.allItemList[this.activeIndex].length}}
            </div>
            <div class="allChoseWrapper">
                全选：
                <div @click.stop="allChose()" :class="allChosed ? 'choseBox choseActive' : 'choseBox'">
                </div>
            </div>
        </div>  
        <div 
        v-for="(item,index) in allItemList[activeIndex]" :key="item.id"
        :class="item.chosed ? 'theItemWrapper itemActive' : 'theItemWrapper'"
        @click="choseClick(index)"
        >
            <div 
            @click.stop="showUpdateInput(index)"
            v-show="!item.updateInputVisible"
            :class="item.chosed ? 'theItemDirection directionActive' : 'theItemDirection'">
                {{item.direction}}
            </div>

            <input 
            :ref="item.id"
            @blur.stop="showUpdateInput(index)"
            @click.stop 
            v-show="item.updateInputVisible" 
            type="text" class="updateInput" 
            v-model="updateInputContent">

            <div class="actionButton">
                <div @click.stop="deleteChosed(index)" class="deleteBox">
                    x
                </div>
                <div @click.stop="choseClick(index)" :class="item.chosed ? 'choseBox choseActive' : 'choseBox'">
                </div>
            </div>
            
        </div>
    </div>
</template>

<script>
export default {
    data(){
        return {
            allItemList:[[],[],[]],
            activeIndex: 0,
            filterStatus: 'all',
            inputContent: '',
            completeNumber: 0,
            chosedItemId: NaN,
            updateInputContent:''
        }
    },
    computed:{
        allChosed(){
            let allChose
            this.allItemList[this.activeIndex].length === 0 ? allChose=false : allChose=true
            this.allItemList[this.activeIndex].forEach((item) => {
                if(item.chosed === false){
                    allChose = false
                    return 
                }
            })
            return allChose
        },
        todoNumber(){
            return this.allItemList[this.activeIndex].length - this.completeNumber
        }
    },
    methods:{
        saveDate(){
            this.getCompleteNumber()
            localStorage.setItem('allItemList',JSON.stringify(this.allItemList))
        },


        choseClick(index){
            this.allItemList[this.activeIndex][index].chosed = !this.allItemList[this.activeIndex][index].chosed
            this.saveDate()
        },
        allChose(){
            const flag = this.allChosed
            this.allItemList[this.activeIndex].forEach((item)=>{
                flag ? (item.chosed = false,item.chosed = false) : (item.chosed = true,item.chosed = true)
            })
            this.getCompleteNumber()
        },


        addItem(){
            if(this.inputContent === ''){
                return
            }
            let id 
            this.allItemList[this.activeIndex].length === 0 ? id = 1 : this.allItemList[this.activeIndex][0].id + 1
            const newItem = {
                direction: this.inputContent,
                chosed: false,
                updateInputVisible: false,
                id
            }
            this.allItemList[this.activeIndex].unshift(newItem)
            this.inputContent = ''
            this.saveDate()
        },
        getCompleteNumber(){
            let number = 0
            this.allItemList[this.activeIndex].forEach((item)=>{
                item.chosed === true ? number+=1 : ''
            })
            this.completeNumber = number
        },


        deleteChosed(index){
            this.allItemList[this.activeIndex].splice(index,1)
            this.saveDate()
        },


        actions(value){
            value === '清空所有' ? 
            this.clearAll() : 
            value === "清空完成" ? 
            this.clearChosed() : ''
        },
        clearChosed(){
            let indexList = []
            this.allItemList[this.activeIndex].forEach((item,index)=>{
                item.chosed ? indexList.push(index) : '' 
            })
            let count = 0
            indexList.forEach((i)=>{
                this.allItemList[this.activeIndex].splice(i-count, 1)
                count += 1
            })
            this.saveDate()
        },
        clearAll(){
            this.allItemList[this.activeIndex].splice(0,this.allItemList[this.activeIndex].length)
            this.saveDate()
        },



        showUpdateInput(index){
            this.allItemList[this.activeIndex][index].updateInputVisible === false ?
            this.updateInputContent = this.allItemList[this.activeIndex][index].direction:
            this.allItemList[this.activeIndex][index].direction = this.updateInputContent
            this.allItemList[this.activeIndex][index].updateInputVisible = !this.allItemList[this.activeIndex][index].updateInputVisible
            this.updateInputContent === '' ? 
            (this.deleteChosed(index),this.saveDate()):''
        },



        filterList(status){
            if(status === 'all'){
                const list  = this.allItemList[this.activeIndex]
                this.allItemList[this.activeIndex] = list
            }else{
                const list  = this.allItemList[this.activeIndex].filter((item)=>{
                    if(status === 'complete'){
                        return item.chosed === true
                    }else{
                        return item.chosed === false
                    }
                })
                this.allItemList[this.activeIndex] = list
                console.log(this.allItemList[this.activeIndex])
            }
            
        },
    },
    watch:{
        filterStatus(status){
            this.filterList(status)
        }
    },
    mounted(){
        if(localStorage.getItem('allItemList')===null){
            localStorage.setItem('allItemList',JSON.stringify(this.allItemList))
        }else{
            this.allItemList = JSON.parse(localStorage.getItem('allItemList'))
        }
        this.getCompleteNumber()
        this.$eventBus.$on('getActiveIndex', (value)=>{this.activeIndex = value})
        this.$eventBus.$on('choseFilterStatus', (value)=>{this.filterStatus = value})
        this.$eventBus.$on('clearCompleted', ()=>{this.clearCompleted()})
        this.$eventBus.$on('switchTab', ()=>{
            this.getCompleteNumber()
            this.inputContent = '' 
        })
        this.$eventBus.$on('actions',this.actions)
    },
    beforeUnmount() {
        this.$eventBus.$off('getActiveIndex')
        this.$eventBus.$off('choseFilterStatus')
        this.$eventBus.$off('clearCompleted')
        this.$eventBus.$off('switchTab')
        this.$eventBus.$off('actions')
    }
}
</script>

<style scoped>
    .theMainWrapper{
        padding: 0.25rem 0.25rem 2rem 0.25rem;
    }
        .theMainWrapper .theItemWrapper{
            display: flex;
            align-items: center;
            padding: 0 0.25rem;
            height: 0.8rem;
            border-bottom: 1px solid rgb(240, 240, 240);
            border-left: 6px solid white;
            transition: all ease 0.3s;
            margin-bottom: 0.25rem;
            justify-content: space-between;
        }
        .theMainWrapper .theItemWrapper:hover{
            border-bottom: 1px solid rgb(0, 205, 205)
        }
            .theItemWrapper .itemActive{
                border-left: 6px solid rgb(0, 205, 205);
            }
            .theItemWrapper .theItemDirection{
                min-width: 5rem;
                transition: all ease 0.3s;
            }
            .theItemWrapper .directionActive{
                text-decoration: line-through;
                opacity: 0.6;
            }
            .actionButton{
                display: flex;
                align-items: center;
            }
            .actionButton .deleteBox{
                border: 1px solid rgb(205, 205, 205);
                width: 0.4rem;
                height: 0.4rem;
                display: flex;
                align-items: center;
                font-size: 0.2rem;
                justify-content: center;
                opacity: 0.6;
            }
            .theMainWrapper .choseBox{
                border: 1px solid rgb(220, 220, 220);
                width: 0.4rem;
                height: 0.4rem;
                border-radius: 5rem;
                margin-left: 0.25rem;
            }
            .theMainWrapper .choseActive{
                background: rgba(0, 205, 205, 0.3);
                border: 1px solid rgba(0, 205, 205, 0.3);
            }

        .theMainWrapper .msgWrapper{
            display: flex;
            font-size: 0.3rem;
            padding: 0.25rem;
            display: flex;
            align-items: center;
            justify-content: space-between;
        }
            .msgWrapper .numberWrapper{
                margin-right: 0.25rem;
            }
            .msgWrapper .allChoseWrapper{
                display: flex;
            }

        .theMainWrapper .addInputWrapper{
            position: relative;
            display: flex;
            align-items: center;
            justify-content: space-between;
            padding: 0.25rem;
        }
            .theMainWrapper .addInput{
                font-size: 0.35rem;
                opacity: 0.6;
            }
        
        .updateInput{
            font-size: 0.35rem;
            opacity: 0.6;
        }
</style>