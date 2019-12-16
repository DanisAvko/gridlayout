<template>
    <v-row>
        <v-col cols="4">
            <v-card>
                <v-card-text>
                    <v-text-field v-model="mask"></v-text-field>
                </v-card-text>
                <v-card-text>
                    <v-select v-model="select" :items="clenght"></v-select>
                </v-card-text>
                <v-card-text>
                    <v-text-field @input="srch" v-model="value" v-mask="mask" label="Value"></v-text-field>
                </v-card-text>
            </v-card>
        </v-col>
        <v-col cols="4">
            <v-card>
                <v-card-text>
                    {{answer }}
                </v-card-text>
                <v-card-actions>
                    <v-btn @click="srch">Найти</v-btn>
                </v-card-actions>
            </v-card>
        </v-col>
    </v-row>
</template>


<script>
    var mas = {
        "1": {
            name: "UATP",
            length: 15,
            rangeMin: 0,
            rangeMAx: 9,
        },
        "2": {
            name: "Mastercard",
            length: 16,
            rangeMin: 2,
            rangeMAx: 7,
            "0": {
                "1": {
                    "4": {
                        name: "Diners Club enRoute",
                        length: 15,
                        rangeMin: 0,
                        rangeMAx: 9,
                    }
                }
            },
            "1": {
                "4": {
                    "9": {
                        name: "Diners Club enRoute",
                        length: 15,
                        rangeMin: 0,
                        rangeMAx: 9,
                    }
                }
            },
            "2": {
                "0": {
                    name: "Mir",
                    length: 16,
                    rangeMin: 0,
                    rangeMAx: 4,
                }
            }
        },
        "3": {
            name: "Diners Club International",
            length: [16, 19],
            rangeMin: 8,
            rangeMAx: 9,
            "0": {
                name: "Diners Club International",
                length: [16, 19],
                rangeMin: 0,
                rangeMAx: 5,
                "9": {
                    name: "Diners Club International",
                    length: [16, 19],
                    rangeMin: 5,
                    rangeMAx: 5,
                },
            },
            "1": {
                name: "China T-Union",
                length: 19,
                rangeMin: 0,
                rangeMAx: 9,
            },
            "4": {
                name: "American Express",
                length: 15,
                rangeMin: 0,
                rangeMAx: 9,
            },
            "5": {
                name: "JCB",
                length: 15,
                rangeMin: 2,
                rangeMAx: 8,
                "7": {
                    name: "JCB",
                    length: 15,
                    rangeMin: 0,
                    rangeMAx: 9,
                    "1": {
                        name: "LankaPay",
                        length: 15,
                        rangeMin: 0,
                        rangeMAx: 9,
                    },
                },
            },
            "6": {
                name: "Diners Club International",
                length: [14, 19],
                rangeMin: 0,
                rangeMAx: 9,
            },
            "7": {
                name: "American Express",
                length: 15,
                rangeMin: 0,
                rangeMAx: 9,
            },
        },
        "4": {},
        "5": {},
        "6": {}
    }


    import {mask} from 'vue-the-mask'

    export default {
        name: "InputCard",
        directives: {
            mask,
        },
        data: () => ({
            mas:mas,
            select:16,
            clenght:[12,13,14,15,16,17,18,19],
            mask: '################',
            value: '',
            answer: '',
            bnks: {}

        }),
        created() {
            import("../mocks/banks.json")
                .then((res) => {
                    var banks = res.banks
                    var tempObj = {
                        bank:this.bnks
                    }
                    banks.forEach(
                        (item, count) => {
                            for (let i = +item.start; i <= +item.end; i++) {
                                for (let j = 0; j < (i.toString()).length; j++) {
                                    if (tempObj.bank.hasOwnProperty([...i.toString()][j])) {
                                        if (j === ((i.toString()).length - 1)) {
                                            tempObj.bank[(i.toString())[j]].name = banks[count].name
                                            tempObj.bank[(i.toString())[j]].len = banks[count].lenght
                                            tempObj= {
                                                bank: this.bnks,
                                            }
                                        } else tempObj.bank = tempObj.bank[(i.toString())[j]]
                                    } else {
                                        if (j === ((i.toString()).length - 1)) {
                                            tempObj.bank[(i.toString())[j]] = {
                                                name: banks[count].name,
                                                len:banks[count].lenght
                                            }
                                            tempObj= {
                                                bank: this.bnks
                                            }
                                        } else {
                                            tempObj.bank[(i.toString())[j]] = {}
                                            tempObj.bank = tempObj.bank[(i.toString())[j]]
                                        }
                                    }

                                }
                            }

                        }
                    )
                })

        },
        watch: {
          select(){
              this.mask="############"
              for (let i= 0;i<this.select-12;i++){
                  this.mask+="#"
              }
          }
        },
        methods: {
            srch() {
                let obj = this.bnks
                for (let i = 0; i <= 6; i++) {
                    if (obj[this.value[i]]) {
                        obj = obj[this.value[i]]
                    } else {
                        break
                    }
                }
                if (obj.name) {
                    this.answer = obj.name
                    if (typeof obj.len === 'object'){
                        this.clenght = obj.len
                        this.select = this.clenght[0]
                    }else {
                        let arr= []
                        arr.push(obj.len)
                        this.clenght = arr
                        this.select=this.clenght[0]
                    }

                } else {
                    this.answer = "Таке нет сори"
                    this.clenght =[12,13,14,15,16,17,18,19]
                }
            },
        }
    }
</script>

<style scoped>

</style>