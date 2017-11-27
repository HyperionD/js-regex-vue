<template>
    <div id="index">
        <div id="input_bar">
            <div id="start">/</div>
            <input />
            <div id="options" @click="showMenu" ><span>/</span> {{ selected_option }} </div>
        </div>
        <div v-show="menu" id="option_menu" ref="optionMenu">
            <div v-for="(item, index) in options" @click="select(index)" @click.prevent @mousedown.stop>{{ item.text }}</div>
        </div>
        <textarea />
    </div>
</template>

<script>
    export default {
        data: function () {
            return {
                options: [
                    {
                        option: "g",
                        text: "全局匹配(g)",
                        selected: true
                    },
                    {
                        option: "m",
                        text: "多行匹配(m)",
                        selected: true
                    },
                    {
                        option: "i",
                        text: "不区分大小写(i)",
                        selected: false
                    }
                ],
                menu: false
            };
        },
        mounted: function () {
            document.addEventListener("mousedown", this.hideMenu);
        },
        computed: {
            selected_option: function () {
                const temp_arr = [];
                for (let value of this.options) {
                    if (value.selected === true) {
                        temp_arr.push(value.option);
                    }
                }
               return temp_arr.join("");
            }
        },
        methods: {
            select: function (index) {
                this.options[index].selected = !this.options[index].selected;
            },
            hideMenu: function () {
                this.menu = false;
            },
            showMenu: function (e) {
                this.menu = true;
                this.$refs.optionMenu.style.top = e.clientY + "px";
                this.$refs.optionMenu.style.left = e.clientX + "px";
                e.preventDefault();
            },
        }
    };
</script>

<style lang="less">
    * {
        margin: 0;
        padding: 0;
    }

    html, body {
        width: 100%;
        height: 100%;
    }

    #index {
        display: flex;
        flex-direction: column;
        align-items: center;
        height: 100%;

        #input_bar {
            display: flex;
            width: 500px;
            margin: 10px;
            border: 1px solid #bdbdbd;
            border-radius: 5px;

            #start {
                color: #bebebe;
                padding-left: 10px;
            }

            input {
                flex-grow: 1;
                border: none;
                outline: none;
                height: 30px;
                padding: 0 5px;
            }

            #options {
                color: #bebebe;
                padding-right: 10px;
                cursor: pointer;

                span {
                    margin-right: 2px;
                }
            }
        }

        textarea {
            width: 500px;
            flex-grow: 1;
            margin: 10px;
            resize: none;
        }

        #option_menu {
            display: flex;
            flex-direction: column;
            z-index: 1;
            position: absolute;
            cursor: pointer;
            border-radius: 2px;
            box-shadow: 0 1px 3px rgba(0,0,0,0.12), 0 1px 2px rgba(0,0,0,0.24);
            background-color: #fff;
            padding: 10px 10px;

            div {
                padding: 10px 10px;
            }
        }
    }
</style>
