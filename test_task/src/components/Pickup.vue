<template>
    <div class="main">
        <form action="" name="pickup-form">
            <div class="radio-group">
                <div class="group radio">
                    <input type="radio" id="address-1" name="address">
                    <label for="address-1" @click="selectAddress()">Пункт Выдачи заказов Песчаная улица, дом 13</label>
                </div>
                <div class="group radio">
                    <input type="radio" id="address-2" name="address">
                    <label for="address-2" @click="selectAddress()">Пункт Выдачи заказов Подсосенский переулок, 11</label>
                </div>
            </div>

            <div id="map"></div>

            <div class="group wide button">
                <input type="submit" id="submit-button" disabled>
            </div>
        </form>
    </div>
</template>

<script>
export default {
    name: 'Pickup',
    data() {
        return {
            iconSize1: 60,
            iconSize2: 60,
            iconOffset1: [-25, -40],
            iconOffset2: [-25, -40],
            myMap: null,
            iconURL: require('@/assets/metka.png'),
        }
    },

    methods: {
        isMobile() {
            Android = function() {
                return navigator.userAgent.match(/Android/i);
            }
            BlackBerry = function() {
                return navigator.userAgent.match(/BlackBerry/i);
            }
            iOS = function() {
                return navigator.userAgent.match(/iPhone|iPad|iPod/i);
            }
            Opera = function() {
                return navigator.userAgent.match(/Opera Mini/i);
            }
            Windows = function() {
                return navigator.userAgent.match(/IEMobile/i);
            }

            return (Android() || BlackBerry() || iOS() || Opera() || Windows());
        },

        initMap() {
            this.myMap = new ymaps.Map("map", {
                center: [55.76, 37.64],
                controls: [],
                zoom: 10,
            });

            this.myMap.controls.add('zoomControl', {
                size: 'small',
                float: 'none',
                position: {
                    bottom: '30px',
                    right: '10px'
                }
            });

            let address1 = new ymaps.Placemark([55.801131, 37.508167], {}, {
                iconLayout: 'default#image',
                iconImageHref: this.iconURL,
                iconImageSize: [this.iconSize1, this.iconSize1],
                iconImageOffset: this.iconOffset1,
            });
            let address2 = new ymaps.Placemark([55.757556, 37.651592], {}, {
                iconLayout: 'default#image',
                iconImageHref: this.iconURL,
                iconImageSize: [this.iconSize2, this.iconSize2],
                iconImageOffset: this.iconOffset2,
            });

            let myCollection = new ymaps.GeoObjectCollection();

            myCollection
                .add(address1)
                .add(address2);

            this.myMap.geoObjects.add(myCollection);

            let centerAndZoom = ymaps.util.bounds.getCenterAndZoom(
                myCollection.getBounds(),
                this.myMap.container.getSize(),
                this.myMap.options.get('projection'),
            );

            this.myMap.setBounds(myCollection.getBounds());
            if (myCollection.getLength() == 2) {
                this.myMap.setCenter(centerAndZoom.center, centerAndZoom.zoom - 1);
            } else if (myCollection.getLength() == 1) {
                this.myMap.setCenter(centerAndZoom.center, 17);
            } else {
                this.myMap.setCenter(centerAndZoom.center, centerAndZoom.zoom);
            }
            console.log(myCollection.getLength());
            this.myMap.behaviors.disable('scrollZoom');

            if (this.isMobile()) {
                this.myMap.behaviors.disable('drag');
            }
        },

        selectAddress() {
            document.getElementById('submit-button').disabled = false;
            setTimeout(() => {
                if (document.getElementById('address-1').checked) {
                    this.iconSize1 = 80;
                    this.iconSize2 = 60;
                    this.iconOffset1 = [-35, -55];
                    this.iconOffset2 = [-25, -40];
                    this.myMap.destroy();
                    this.initMap();
                } else if (document.getElementById('address-2').checked) {
                    this.iconSize2 = 80;
                    this.iconSize1 = 60;
                    this.iconOffset2 = [-35, -55];
                    this.iconOffset1 = [-25, -40];
                    this.myMap.destroy();
                    this.initMap();
                }
            }, 100);
        }
    },

    mounted() {
        ymaps.ready(this.initMap());
    }
}
</script>

<style lang="less" scoped>
    @font: 'Montserrat', sans-serif;
    @selected-radio-color: #212bff;
    @radio-background: white;
    @radio-border-color: #b8bed8;
    @radio-disabled-color: #edeeef;

    .main {
        font-family: @font;
        font-weight: 300;
        font-size: 14px;
    }

    .group {
        width: calc(50% - 20px);
        margin-top: 30px;
    }

    .radio {
        margin-right: 40px;
        display: inline-block;
        float: left; 
    }

    .radio-group {
        display: inline-block;
        width: 100%;
    }

    .radio-group :last-child {
        margin-right: 0;
    }

    .wide {
        width: 100%;
    }

    .button {
        text-align: right;
        margin-top: 0;
    }

    input[type="radio"] {
        position: absolute;
        z-index: -1;
        opacity: 0;
        margin: 2px 0 0 0px;
    }
    input[type="radio"] + label {
        position: relative;
        padding: 0 0 0 30px;
        cursor: pointer;
        display: inline-block;
    }
    input[type="radio"] + label::before {
        content: '';
        position: absolute;
        top: -1px;
        left: -2px;
        width: 18px;
        height: 18px;
        border: 1px solid @radio-border-color;
        border-radius: 50%;
        background: @radio-background;
    }
    input[type="radio"] + label::after {
        content: '';
        position: absolute;
        top: 6px;
        left: 5px;
        width: 6px;
        height: 6px;
        border-radius: 50%;
        background: @selected-radio-color;
        opacity: 0;
    }
    input[type="radio"]:checked + label::before {
        border-color: @selected-radio-color;
    }
    input[type="radio"]:checked + label::after {
        opacity: 1;
    }
    input[type="radio"] {
        +label:hover {
            color: @selected-radio-color;
        }
        +label:hover::before {     
            border-color: @selected-radio-color;
        }
    } 
    input[type="radio"]:disabled {
        +label {
            color: @radio-border-color;
            cursor: default;
        }
        +label::before {
            border-color: @radio-disabled-color;
            background-color: @radio-disabled-color;
        }
    }

    #map {
        width: 100%;
        height: 500px;
        margin: 30px 0;
    }
</style>