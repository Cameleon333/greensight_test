<template>
    <div id="main">
        <h2>Выберите способ доставки</h2>
        <div class="tab-panel">
            <div class="tabs" :class="{ activeTab: tab.isActive}" v-for="(tab, index) in tabList" :key="index" @click="selectTab(index)"><span class="tab-text">{{ tab.text }}</span></div>
        </div>
        <div class="tab-content">
        </div>
    </div>
</template>

<script>
export default {
    name: 'TabsBar',
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
    @font: sans-serif;
    @selectedTextColor: #212bff;

    #main {
        min-width: 320px;
        max-width: 1200px;
        width: 100%;
        margin: auto;
    }

    h2 {
        font-family: @font;
        font-size: 32px;
        font-weight: 300;
        line-height: 32px;
        margin-top: 40px;
        letter-spacing: 2px;
    }

    .tabs {
        width: 140px;
        height: 70px;
        background-color: #edeeef;
        color: #8288a2;
        display: inline-block;
        border-radius: 5px 5px 0 0;
        position: relative;
        cursor: pointer;
    }
    .tabs:hover {
        color: @selectedTextColor;
    }

    .tab-text {
        display: block;
        font-family: @font;
        font-size: 16px;
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
        border-bottom: 68px solid #dadbdc;
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
        border-bottom: 68px solid #edeeef;
        position: absolute;
        bottom: 0;
        right: -27.5px;
        z-index: -1;
    }

    // For selected tab
    .activeTab {
        background-color: white;
        color: @selectedTextColor;
        z-index: 21;
    }
    .activeTab::after {
        border-bottom: 69px solid white;
    }

    .tab-content {
        width: 100%;
        background-color: white;
        padding: 40px 30px;
    }
</style>