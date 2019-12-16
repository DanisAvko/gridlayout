<template>
    <div>
        <div>
            <div>Columns {{column}}</div>
            <div>Width {{panelWidth}}</div>
<!--            <div><pre>layout {{layout}}</pre></div>-->
        </div>
        <div>
            <v-combobox style="width: 300px"
                        v-model="select"
                        :items="items"
                        label="Number"
            ></v-combobox>
        </div>
        <div >
            <div id="ell" class="layer" >
                <grid-layout
                        :row-height="50"
                        :is-draggable="false"
                        :is-resizable="true"
                        :vertical-compact="true"
                        :use-css-transforms="true"
                        :style="'width:'+panelWidth[i] +'%'"
                        v-for="(part,i) in layout"
                        :layout="part"
                        :col-num="column[i]+1"
                        :key="i"
                >
                    <grid-item v-for="(item,j) in part"
                               :static="item.static"
                               :x="item.x"
                               :y="item.y"
                               :w="item.w"
                               :h="item.h"
                               :i="item.i"
                               :key="j"
                               @resize="resizeEvent"
                               @resized="resizedEvent"
                    >

                        <v-card width="100%" height="100%">
                            <v-card-title @click="deleteCard(i,j)" class="float-right"><v-btn icon> <v-icon>mdi-close</v-icon></v-btn></v-card-title>
                            <span class="text">{{itemTitle(item)}}</span>
                        </v-card>
                    </grid-item>
                </grid-layout>


            </div>
        </div>
    </div>
</template>

<script>
    var testLayout = [
        {"x": 0, "y": 0, "w": 2, "h": 3, "i": 0, static: false},
        {"x": 2, "y": 0, "w": 2, "h": 3, "i": 1, static: false},
        {"x": 4, "y": 0, "w": 2, "h": 3, "i": 2, static: false},
        {"x": 6, "y": 0, "w": 2, "h": 3, "i": 3, static: false},
        {"x": 8, "y": 0, "w": 2, "h": 3, "i": 4, static: false},
        {"x": 10, "y": 0, "w": 2, "h": 3, "i": 5, static: false},
        {"x": 12, "y": 0, "w": 2, "h": 3, "i": 6, static: false},
        {"x": 14, "y": 0, "w": 2, "h": 3, "i": 7, static: false},
        {"x": 16, "y": 0, "w": 2, "h": 3, "i": 8, static: false},
        {"x": 18, "y": 0, "w": 2, "h": 3, "i": 9, static: false},
        {"x": 0, "y": 0, "w": 2, "h": 3, "i": 10, static: false},
        {"x": 2, "y": 0, "w": 2, "h": 3, "i": 11, static: false},
        {"x": 4, "y": 0, "w": 2, "h": 3, "i": 12, static: false},
        {"x": 6, "y": 0, "w": 2, "h": 3, "i": 13, static: false},
        {"x": 8, "y": 0, "w": 2, "h": 3, "i": 14, static: false},
        {"x": 10, "y": 0, "w": 2, "h": 3, "i": 15, static: false},
        {"x": 12, "y": 0, "w": 2, "h": 3, "i": 16, static: false},
        {"x": 14, "y": 0, "w": 2, "h": 3, "i": 17, static: false},
        {"x": 16, "y": 0, "w": 2, "h": 3, "i": 18, static: false},
        {"x": 18, "y": 0, "w": 2, "h": 3, "i": 19, static: false}
    ];
    import VueGridLayout from 'vue-grid-layout';

    export default {
        data: () => ({
            sqr: testLayout,
            column: [],
            panelWidth: [],
            select: 4,
            items: [4, 5, 6, 7, 8, 9, 10],
            layout: null
        }),
        components: {
            GridLayout: VueGridLayout.GridLayout,
            GridItem: VueGridLayout.GridItem
        },
        computed: {
            rowCount() {

                return Math.ceil(this.sqr.length / this.select)
            },
            initCountColumns(){
                return 3*this.select
            },
            blockWidth(){
                return 100/this.initCountColumns
            },
        },
        watch: {
            select: {
                immediate: true,
                handler: function () {
                    this.column=Array(this.rowCount)
                    this.panelWidth=Array(this.rowCount)
                    var res = []
                    for (let i = 0; i < this.rowCount; i++) {
                        let resPart = testLayout.slice(i * this.select, this.select * (i + 1))
                        res.push(resPart)
                    }
                    res.forEach((item, index) => {
                        this.column[index] = this.initCountColumns
                        this.panelWidth[index] = 100
                        for (let i = 0; i < item.length; i++) {
                            item[i].w = this.column[index] / this.select
                            item[i].x = i * this.column[index] / this.select
                            item[i].y = index * 3
                            // eslint-disable-next-line no-console
                            console.log(item[i].w)
                        }
                    })
                    this.layout = res
                }
            }

        },
        methods: {
            itemTitle(item) {
                var result = item.i;
                if (item.static) {
                    result += " - Static";
                }
                return result;
            },
            resizeEvent(j, newH, newW) {
                var masInd = Math.floor(j / this.select)
                var elInd = j % this.select
                var coef = newW - this.layout[masInd][elInd].w
                var masIndLen = this.layout[masInd].length
                if (newW !== this.layout[masInd][elInd].w) {
                    for (let i = elInd + 1; i < masIndLen; i++) {
                        this.layout[masInd][i].x += coef
                    }
                    if (elInd !== masIndLen - 1) {
                        if (this.layout[masInd][masIndLen - 1].x + this.layout[masInd][masIndLen - 1].w >= this.initCountColumns) {
                            this.column[masInd] = this.layout[masInd][masIndLen - 1].x + this.layout[masInd][masIndLen - 1].w
                            this.panelWidth[masInd] = this.column[masInd] * this.blockWidth

                        }
                    } else {
                        if (this.layout[masInd][masIndLen - 1].x + newW >= (this.column[masInd]+1)) {
                            this.column[masInd] = this.layout[masInd][masIndLen - 1].x + newW
                            this.panelWidth[masInd] = this.column[masInd] * this.blockWidth
                        }
                    }


                }
            },
            resizedEvent(j,newH,newW) {
                var masInd = Math.floor(j / this.select)
                var masIndLen = this.layout[masInd].length
                var elInd = j % this.select
                if (elInd === masIndLen - 1) {
                    this.$set(this.column, masInd,  this.layout[masInd][masIndLen - 1].x + newW)
                    this.$set(this.panelWidth, masInd, this.column[masInd] *this.blockWidth)
                }
            },
            deleteCard(){
                this.layout[this.rowCount-1].splice(this.layout[this.rowCount-1].length-1,1)
                this.sqr.splice(testLayout.length - 1,1)
                // eslint-disable-next-line no-console
                console.log(this.rowCount,testLayout.length)

            }
        },
        mounted() {
            this.column=Array(this.rowCount)
            this.panelWidth=Array(this.rowCount)
            this.layout.forEach((item, index) => {
                this.column[index] = this.initCountColumns
                this.panelWidth[index]=100
                for (let i = 0; i < item.length; i++) {
                    item[i].w = this.column[index] / this.select
                    item[i].x = i * this.column[index] / this.select
                    item[i].y = index * 3
                }
            })
        }
    }


</script>

<style scoped>
    .layer {
        overflow: auto;
    }

</style>