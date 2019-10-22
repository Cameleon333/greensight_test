<template>
    <div id="main">
        <h2>Выберите способ доставки</h2>
        <div class="tab-panel">
            <div class="tabs" :class="{ activeTab: tab.isActive}" v-for="(tab, index) in tabList" :key="index" @click="selectTab(index)"><span class="tab-text">{{ tab.text }}</span></div>
        </div>
        <div class="tab-content">
            <delivery v-if="tabList[0].isActive"/>
        </div>
    </div>
</template>

<script>
import Delivery from "./Delivery.vue"

export default {
    name: 'TabsBar',
    components: {
        Delivery,
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
            currentTab: 0,
        }
    },

    methods: {
        selectTab(index) {
            this.tabList.forEach((element) => {
                element.isActive = false;
            });
            this.tabList[index].isActive = true;
        }
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

    #main {
        min-width: 320px;
        max-width: 1200px;
        margin: 0 auto;
        // display: inline-block;
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
    .activeTab {
        background-color: white;
        color: @selected-text-color;
        z-index: 21;
    }
    .activeTab::after {
        border-bottom: 69px solid white;
    }

    .tab-content {
        min-width: 320px;
        max-width: 1200px;
        background-color: white;
        padding: 20px 30px;
        // display: inline-block;
    }


</style>

<style lang="less">
    // For submit button
    @button-color: #212bff;
    @button-color-disabled: #edeeef;
    @button-disabled-text-color: #b8bed8;

    input[type="submit"] {
        height: 60px;
        width: 80px;
        border-radius: 30px;
        background-color: @button-color;
        color: white;
        cursor: pointer;
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