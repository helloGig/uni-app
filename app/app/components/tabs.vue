
<template>
    <view 
        class="tabs"
    >
        <scroll-view class="active-switch" scroll-x :scroll-into-view="id" scroll-with-animation>
            <view class="switch-container" >
                <view 
                v-for="(item,index) in TabList" :key="index"
                :class="['active-item',currentTab==index&&'focus',TabList.length==3&&'fix']"
                @tap="tabChange(index)"
                :id='`tab_${index}`'
                >
                    <view class="item">{{item.title}}</view>
                </view>
                <view class="focus-line"
                    :class="[TabList.length==3&&'fix']"
                    :style="{transform:transformX}"
                >
                </view>
            </view>
        </scroll-view>
        <view class="tab-pane-view"
            @touchstart='touchstart'
            @touchend='touchend'
        >
            <view 
              class="tab-pane-group"
              :style="{transform:transformXx}"  
            >
            <slot></slot>
            </view>
        </view>
    </view>    
</template>
<script>
export default {
    name:'Tabs',
    data(){
        return {
            id:'tab_0',
            start:0
        }
    },
    props:{
        TabList:{
            default:()=>{
                return []
            },
            type:Array
        },
        currentTab:{
            default:0,
            type:Number
        }
    },
    computed:{
        transformX(){
            let currentTab = this.currentTab;
            return `translate3d(${currentTab*100}%, 0px, 0px)`
        },
        transformXx(){
            let currentTab = this.currentTab;
            return `translate3d(-${currentTab*100}%, 0px, 0px)`
        }
    },
    methods:{
        tabChange(index){
            if(this.currentTab!=index){
                console.log(`emit:${index}`);
                this.$emit('tabs',index);         
                this.id = `tab_${index}`
            }
        },
        touchstart(e){
            this.start = e.touches[0].clientX;
        },
        touchend(e){
            let end = e.changedTouches[0].clientX;
            if(end-this.start>100&&this.currentTab>=1){
                this.tabChange(this.currentTab-1)
            }else if(this.start-end>100&&this.currentTab<this.TabList.length-1){
                this.tabChange(this.currentTab+1)
            }
        }
    }
}
</script>
<style lang="stylus">
.tabs
    position relative
    height 100vh
    display flex
    flex-direction column
    .active-switch
        overflow scroll
        .switch-container
            position relative
            display flex
            flex-direction row
            .focus-line
                flex 1
                width 160upx
                position absolute
                bottom 0
                border-bottom 4upx solid #f07
                transition 0.3s
                &.fix
                    width 250upx
        .active-item
            position relative
            min-width 160upx
            width 160upx
            flex 1
            height 100upx
            transition .3s
            background-color: #fff
            color #000
            text-align: center
            display: flex 
            flex-direction: column
            justify-content: space-around
            border-bottom 1upx solid rgba(0,0,0,0.5)
            &.focus
                background #fff
                color #f07
                transition-duration: .3s
            &.fix
                width 250upx
            .item
                // width: 220upx
                padding: 0 5upx
                overflow hidden
                font-size: 28upx
    .tab-pane-view
        overflow hidden
        background-color: #f7f7f7
        flex 1
        .tab-pane-group
            display: block;
            white-space: nowrap;
            -webkit-transition: all .3s;
            transition: all .3s;
            width: 100%;
            overflow: visible;
            will-change: transform,left,top;
            min-height 100upx
            height 100%
            .tab-pane-item
                width: 100%;
                min-height 100upx
                display: inline-block
                white-space: initial;
                vertical-align: top;
                font-size: 24upx;
                box-sizing: border-box;
                overflow: auto
</style>
