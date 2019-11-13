<template>
    <div id="app">
        <div class="wrapper">
            <h4 class="title">MULTISELECT SEARCH DROPDOWN STATES</h4>
            <div class="select-box" :class="{ 'open' : willDropdownListVisible }">
                <div class="select-box-container" :class="{ 'error-container' : showError }">
                    <div class="select-box-label">Account category<span>*</span></div>
                    <div class="select-box-input-container">
                        <span class="selected-item" v-for="(selectedValue, key) in selectedValues" v-bind:key="key" @click="removeSelectedValue(selectedValue)">
                            {{selectedValue}} <span class="clear-btn"><img src="assets/img/close.png"/></span>
                        </span>
                        <input class="select-box-input" v-if="actionType == 'search'" ref="input" @keyup="searchThisValue" @blur="blurInputField" @focus="showDropdownList" v-model="searchValue" placeholder="Type to search or choose from menu" type="text"/>
                        <span v-if="!willDropdownListVisible" class="select-box-arrow down" @click="showDropdownList"></span>
                        <span v-if="willDropdownListVisible" class="select-box-arrow up" @click="hideDropdownList"></span>
                    </div>
                </div>
                <div>
                    <span v-if="showError" class="error">Please, select a valid account category</span>
                    <span v-else class="help-text">E.g. makeup artist, marketing agency, etc.</span>
                </div>
                <div class="select-box-dropdown" :class="{ 'd-block' : willDropdownListVisible }">
                    <ul class="selected-cat" v-if="willSelectedValueVisible">
                        <li v-for="(selectedValue, key) in selectedValues" v-bind:key="key" v-bind:style="{ 'padding-left': key*15 + 15 + 'px' }" @click="removeSelectedValue(selectedValue)">
                            {{selectedValue}} <span class="select-box-checked-icon checked"><img src="assets/img/checked.png"/></span>
                        </li>
                    </ul>
                    <ul class="cat-list" v-if="willDropdownListVisible">
                        <li v-for="(dropdownList, key) in dropdownLists" v-bind:key="key"  @click="valueSelected(dropdownList.name)" v-bind:style="{ 'padding-left': selectedValues.length*15 + 15 + 'px' }">
                            {{dropdownList.name}} <span class="select-box-checked-icon unchecked"><img src="assets/img/unchecked.png"/></span>
                        </li>
                    </ul>
                    <ul class="cat-search" v-if="actionType == 'search' && nothingFound">
                        <li v-bind:style="{ 'padding-left': selectedValues.length*15 + 15 + 'px' }">
                            Oops, nothing found <span class="clear-result" @click="clearInput()">clear entry</span>
                        </li>
                    </ul>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
    import _ from 'lodash'; // eslint-disable-line

    export default {
        name: 'app',
        data () {
            return {
                dataResults : [
                    {
                        name : 'Category 1',
                        data : [
                            {
                                name : 'Sub Category 11 Item',
                                data: [
                                    {
                                        name: 'Sub Sub Category 111',
                                        data: []
                                    },
                                    {
                                        name: 'Sub Sub Category 112',
                                        data: [
                                            {
                                                name : 'Other',
                                                data : []
                                            }
                                        ]
                                    },
                                    {
                                        name: 'Sub Sub Category 113',
                                        data: [
                                            {
                                                name : 'Other',
                                                data : []
                                            }
                                        ]
                                    }
                                ]
                            },
                            {
                                name : 'Sub Category 12 Item',
                                data: [
                                    {
                                        name: 'Sub Sub Category 121',
                                        data: [
                                            {
                                                name : 'Other',
                                                data : []
                                            }
                                        ]
                                    },
                                    {
                                        name: 'Sub Sub Category 122',
                                        data: []
                                    },
                                    {
                                        name: 'Sub Sub Category 123',
                                        data: [
                                            {
                                                name : 'Other',
                                                data : []
                                            }
                                        ]
                                    },
                                    {
                                        name : 'Other',
                                        data : []
                                    }
                                ]
                            },
                            {
                                name : 'Sub Category 13 Item',
                                data: [
                                    {
                                        name: 'Sub Sub Category 131',
                                        data: [
                                            {
                                                name : 'Other',
                                                data : []
                                            }
                                        ]
                                    },
                                    {
                                        name: 'Sub Sub Category 132',
                                        data: [
                                            {
                                                name : 'Other',
                                                data : []
                                            }
                                        ]
                                    },
                                    {
                                        name: 'Sub Sub Category 133',
                                        data: [
                                            {
                                                name : 'Other',
                                                data : []
                                            }
                                        ]
                                    },
                                    {
                                        name : 'Other',
                                        data : []
                                    }
                                ]
                            },
                            {
                                name : 'Other',
                                data : []
                            }
                        ]
                    },
                    {
                        name : 'Category 2',
                        data : [
                            {
                                name : 'Sub Category 21 Item',
                                data: [
                                    {
                                        name: 'Sub Sub Category 211',
                                        data: [
                                            {
                                                name : 'Other',
                                                data : []
                                            }
                                        ]
                                    },
                                    {
                                        name: 'Sub Sub Category 212',
                                        data: [
                                            {
                                                name : 'Other',
                                                data : []
                                            }
                                        ]
                                    },
                                    {
                                        name: 'Sub Sub Category 213',
                                        data: [
                                            {
                                                name : 'Other',
                                                data : []
                                            }
                                        ]
                                    },
                                    {
                                        name : 'Other',
                                        data : []
                                    }
                                ]
                            },
                            {
                                name : 'Sub Category 22 Item',
                                data: [
                                    {
                                        name: 'Sub Sub Category 221',
                                        data: [
                                            {
                                                name : 'Other',
                                                data : []
                                            }
                                        ]
                                    },
                                    {
                                        name: 'Sub Sub Category 222',
                                        data: [
                                            {
                                                name : 'Other',
                                                data : []
                                            }
                                        ]
                                    },
                                    {
                                        name: 'Sub Sub Category 223',
                                        data: [
                                            {
                                                name : 'Other',
                                                data : []
                                            }
                                        ]
                                    },
                                    {
                                        name : 'Other',
                                        data : []
                                    }
                                ]
                            },
                            {
                                name : 'Sub Category 23 Item',
                                data: [
                                    {
                                        name: 'Sub Sub Category 231',
                                        data: [
                                            {
                                                name : 'Other',
                                                data : []
                                            }
                                        ]
                                    },
                                    {
                                        name: 'Sub Sub Category 232',
                                        data: [
                                            {
                                                name : 'Other',
                                                data : []
                                            }
                                        ]
                                    },
                                    {
                                        name: 'Sub Sub Category 233',
                                        data: [
                                            {
                                                name : 'Other',
                                                data : []
                                            }
                                        ]
                                    },
                                    {
                                        name : 'Other',
                                        data : []
                                    }
                                ]
                            },
                            {
                                name : 'Other',
                                data : []
                            }
                        ]
                    },
                    {
                        name : 'Category 3',
                        data : [
                            {
                                name : 'Sub Category 31 Item',
                                data: [
                                    {
                                        name: 'Sub Sub Category 311',
                                        data: [
                                            {
                                                name : 'Other',
                                                data : []
                                            }
                                        ]
                                    },
                                    {
                                        name: 'Sub Sub Category 312',
                                        data: [
                                            {
                                                name : 'Other',
                                                data : []
                                            }
                                        ]
                                    },
                                    {
                                        name: 'Sub Sub Category 313',
                                        data: [
                                            {
                                                name : 'Other',
                                                data : []
                                            }
                                        ]
                                    },
                                    {
                                        name : 'Other',
                                        data : []
                                    }
                                ]
                            },
                            {
                                name : 'Sub Category 32 Item',
                                data: [
                                    {
                                        name: 'Sub Sub Category 321',
                                        data: [
                                           {
                                                name : 'Other',
                                                data : []
                                            }
                                        ]
                                    },
                                    {
                                        name: 'Sub Sub Category 322',
                                        data: [
                                            {
                                                name : 'Other',
                                                data : []
                                            }
                                        ]
                                    },
                                    {
                                        name: 'Sub Sub Category 323',
                                        data: [
                                            {
                                                name : 'Other',
                                                data : []
                                            }
                                        ]
                                    },
                                    {
                                        name : 'Other',
                                        data : []
                                    }
                                ]
                            },
                            {
                                name : 'Sub Category 33 Item',
                                data: [
                                    {
                                        name: 'Sub Sub Category 331',
                                        data: [
                                            {
                                                name : 'Other',
                                                data : []
                                            }
                                        ]
                                    },
                                    {
                                        name: 'Sub Sub Category 332',
                                        data: [
                                            {
                                                name : 'Other',
                                                data : []
                                            }
                                        ]
                                    },
                                    {
                                        name: 'Sub Sub Category 333',
                                        data: [
                                            {
                                                name : 'Other',
                                                data : []
                                            }
                                        ]
                                    },
                                    {
                                        name : 'Other',
                                        data : []
                                    }
                                ]
                            },
                            {
                                name : 'Other',
                                data : []
                            }
                        ]
                    },
                    {
                        name : 'Other',
                        data : []
                    }
                ],
                dropdownLists : [],
                selectedValues: [],
                selectedTopArray: {},
                lastValue: '',
                actionType: 'search',
                searchValue: '',
                willDropdownListVisible: false,
                willSelectedValueVisible: true,
                allArray: {},
                nothingFound: false,
                showError: false
            }
        },
        mounted () {
            this.dropdownLists = this.dataResults;

            this.flatten(this.dataResults);
        },
        methods: {
            flatten(data) {
                var that = this;
                data.forEach(function (element){
                    if(Array.isArray(element.data)) {
                        that.$set(that.allArray, element.name, element);
                        that.flatten(element.data);
                    } else {
                        that.$set(that.allArray, element.name, element);
                    }
                });
            },
            valueSelected (key) {
                this.selectedValues.push(key);
                this.lastValue = key;
                this.actionType = 'check';
                this.showError = false;
                this.nothingFound = false;
                this.searchValue = '';

                let currentDropdownList = _.filter(this.dropdownLists, (dropdownList) => {
                    return dropdownList.name === key;
                });
                this.dropdownLists = currentDropdownList[0].data;

                this.$set(this.selectedTopArray, key, this.dropdownLists);

                // If no data available in array hide selected values
                if(key === 'Other' || this.dropdownLists.length === 0)
                {
                    this.willSelectedValueVisible = false;

                }
            },
            removeSelectedValue (key) {

                let searchedKey = _.findIndex(this.selectedValues, function(selectedValue) { return selectedValue == key; });
                this.selectedValues = _.filter(this.selectedValues, (selectedValue, selectedValueKey) => {
                    return selectedValueKey <= searchedKey;
                });

                let lastValueKey = this.selectedValues.slice(-1).pop();

                // We Only remove element when it is last element
                if(key === lastValueKey)
                {
                    this.selectedValues = _.filter(this.selectedValues, (selectedValue) => {
                      return selectedValue != key;
                    });

                    let newLastValueKey = this.selectedValues.slice(-1).pop();

                    // If no value in selected value then
                    // Show all elements in list
                    if(newLastValueKey === undefined)
                    {
                        this.dropdownLists = this.dataResults;
                        this.actionType = 'search';
                    } else {
                        this.lastValue = lastValueKey;

                        this.dropdownLists = this.selectedTopArray[newLastValueKey];
                    }

                    this.willSelectedValueVisible = true;
                }
            },
            searchThisValue() {
                let currentDropdownList = [];
                this.actionType = 'search';
                this.nothingFound = false;
                this.showError = false;
                var that = this;
                _.forEach(this.allArray, function(allArrayValue) {
                    if(allArrayValue.name.toLowerCase().includes(that.searchValue.toLowerCase()))
                    {
                        currentDropdownList.push(allArrayValue);
                    }
                });



                if(currentDropdownList.length > 0)
                {
                    this.dropdownLists = currentDropdownList;
                } else {
                    this.dropdownLists = [];
                    this.nothingFound = true;
                }

                if(this.searchValue == '') {
                    this.dropdownLists = this.dataResults;
                }

                console.log(this.searchValue); // eslint-disable-line
            },
            showDropdownList () {
                this.willDropdownListVisible = true;
            },
            hideDropdownList () {
                if(this.selectedValues.length === 0)
                {
                    this.willDropdownListVisible = false;
                } else {
                    this.willDropdownListVisible = true;
                }
            },
            blurInputField () {
                if(this.searchValue != '')
                {
                    this.showError = true;
                    this.nothingFound = false;
                }else if(this.selectedValues.length === 0 && this.willDropdownListVisible)
                {
                    this.$nextTick(() => this.$refs.input.focus());
                }
            },
            clearInput () {
                this.searchValue = '';
                this.searchThisValue();
            }
        }
    }
</script>

<style lang="css">
    @import './assets/fonts/fonts.css';
    @import './assets/css/style.css';
</style>
