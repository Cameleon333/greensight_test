<template>
    <div id="main">
        <h2>Выберите способ доставки</h2>
        <div v-if="!smallScreen">
            <div class="tab-panel">
                <div class="tabs" :class="{ 'active-tab': tab.isActive }" v-for="(tab, index) in tabList" :key="index" @click="selectTab(index)"><span class="tab-text">{{ tab.text }}</span></div>
            </div>
            <div class="tab-content">
                <delivery v-if="tabList[0].isActive"/>
                <pickup v-if="tabList[1].isActive"/>
            </div>
        </div>
        <div v-else id="small">
            <div class="small-tab">
                    <span class="small-tab-text" :class="{ 'small-active-tab': tabList[0].isActive }" @click="selectTab(0)">{{ tabList[0].text }}</span>
                    <div class="arrow"></div>
            </div>
            <div class="small-tab-content" v-if="tabList[0].isActive">
                <delivery/>
            </div>
            <div class="small-tab" :class="{ 'fix-double-border': tabList[1].isActive }">
                    <span class="small-tab-text" :class="{ 'small-active-tab': tabList[1].isActive }" @click="selectTab(1)">{{ tabList[1].text }}</span>
                    <div class="arrow"></div>
            </div>
            <div class="small-tab-content" v-if="tabList[1].isActive">
                <pickup/>
            </div>
        </div>
    </div>
</template>

<script>
import Delivery from "./Delivery.vue"
import Pickup from "./Pickup.vue"

export default {
    name: 'TabsBar',
    components: {
        Delivery,
        Pickup,
    },
    data() {
        return {
            tabList: [
                {
                    text: 'Доставка',
                    isActive: true,
                },
                {
                    text: 'Самовывоз',
                    isActive: false,
                },
            ],
            // currentTab: 0,
            screenWidth: 0,
            smallScreen: false,
        }
    },

    methods: {
        selectTab(index) {
            // alert("hello");
            this.tabList.forEach((element) => {
                element.isActive = false;
            });
            this.tabList[index].isActive = true;
        },

        updateScreenWidth() {
            this.screenWidth = window.innerWidth;
            if (this.screenWidth < 768) this.smallScreen = true;
            else this.smallScreen = false;
        }
    },

    mounted() {
        this.updateScreenWidth();
    },

    created() {
        window.addEventListener('resize', this.updateScreenWidth);
    }
}
</script>

<style lang="less" scoped>
    @font: 'Montserrat', sans-serif;
    @selected-text-color: #212bff;
    @tabs-background-color: #edeeef;
    @tabs-text-color: #8288a2;
    @tabs-before-border-bottom-color: #dadbdc;
    @tabs-after-border-bottom-color: #edeeef;
    @border-color: #b8bed8;

    #main {
        min-width: 320px;
        max-width: 1200px;
        margin: 0 auto;
        vertical-align: top;
    }

    h2 {
        font-family: @font;
        font-size: 32px;
        font-weight: 400;
        line-height: 32px;
        margin-top: 40px;
        margin-bottom: 40px;
    }

    .tabs {
        width: 140px;
        height: 70px;
        background-color: @tabs-background-color;
        color: @tabs-text-color;
        display: inline-block;
        border-radius: 5px 5px 0 0;
        position: relative;
        cursor: pointer;
    }
    .tabs:hover {
        color: @selected-text-color;
    }

    .tab-text {
        display: block;
        font-family: @font;
        font-size: 14px;
        font-weight: 300;
        margin-left: 30px;
        margin-top: 28px;
    }
    
    .tabs::before {
        content: '';
        display: block;
        width: 0;
        height: 0;
        border-right: 30px solid transparent;
        border-bottom: 68px solid @tabs-before-border-bottom-color;
        position: absolute;
        bottom: 0;
        right: -29.5px;
        z-index: -1;
    }
    .tabs::after {
        content: '';
        display: block;
        width: 0;
        height: 0;
        border-right: 30px solid transparent;
        border-bottom: 68px solid @tabs-after-border-bottom-color;
        position: absolute;
        bottom: 0;
        right: -27.5px;
        z-index: -1;
    }

    // For selected tab
    .active-tab {
        background-color: white;
        color: @selected-text-color;
        z-index: 21;
    }
    .active-tab::after {
        border-bottom: 69px solid white;
    }

    .tab-content {
        min-width: 290px; //320
        max-width: 1170px; //1200
        background-color: white;
        padding: 20px 30px;
    }

    //for small screen width
    .small-tab {
        position: relative;
        min-width: 320px;
        width: calc(100% - 8px);
        border-top: 1px solid @border-color;
        border-bottom: 1px solid @border-color;
        background-color: white;
        color: @tabs-text-color;
        margin: auto;
    }
    .small-tab:hover {
        color: @selected-text-color;
        cursor: pointer;
        .arrow {
            border-color: @selected-text-color;
        }
    }

    .small-tab-text {
        display: block;
        font-family: @font;
        font-size: 14px;
        font-weight: 300;
        margin-left: 15px;
        margin-top: 15px;
        margin-bottom: 15px;
    }

    .fix-double-border {
        border-top: 1px solid white;
        .small-tab-text {
            margin-top: 14px;
        }
    }

    .arrow {
        width: 0;
        height: 0;
        padding: 3px;
        border-top: 2px solid @border-color;
        border-left: 2px solid @border-color;
        position: absolute;
        right: 30px;
        top: 20px;
        transform: rotate(-135deg);
    }

    .small-active-tab {
        color: @selected-text-color;
        + .arrow {
            transform: rotate(45deg);
            border-color: @selected-text-color;
        }
    }

    .small-tab-content {
        min-width: 290px; //320
        width: calc(100% - 38px);
        background-color: white;
        padding: 15px;
        margin: auto;
    }

</style>

<style lang="less">
    // For submit button
    @button-color: #212bff;
    @button-color-disabled: #edeeef;
    @button-disabled-text-color: #b8bed8;

    input[type="submit"] {
        height: 60px;
        width: 200px;
        border-radius: 30px;
        background-color: @button-color;
        color: white;
        cursor: pointer;
        font-size: 16px;
    }
    input[type="submit"]:hover {
        background-color: @button-color - #111111;
    }
    input[type="submit"]:active {
        background-color: @button-color - #333333;
    }
    input[type="submit"]:disabled {
        background-color: @button-color-disabled;
        color: @button-disabled-text-color;
        cursor: default;
    }
</style>