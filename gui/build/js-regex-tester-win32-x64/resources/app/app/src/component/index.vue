<template>
    <div id="index">
        <div id="input_bar">
            <div id="start">/</div>
            <input v-model="reg_string" size="18px" placeholder="输入正则"/>
            <div id="options" @click="showMenu" >/ {{ selected_option }}</div>
        </div>
        <div id="regex">
            <textarea v-model="text" placeholder="输入测试文本"></textarea>
            <div id="result" v-html="matchText"></div>
        </div>
        <div v-show="menu" id="option_menu" ref="optionMenu">
            <div v-for="(item, index) in options" @click="select(index)" @click.prevent @mousedown.stop>
                {{ item.text }}
                <span v-show="item.selected">&#10003</span>
            </div>
        </div>
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
                menu: false,
                reg_string: "",
                text: ""
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
            },
            reg: function () {
                return new RegExp(this.reg_string, this.selected_option);
            },
            matchText: function () {
                if (this.reg_string !== "") {
                    const text_add_tag = this.text.replace(this.reg, function (match) {
                        return `<span class="matched">${match}</span>`;
                    });

                    return text_add_tag.replace(/[\r\n]/gm, "<br>");
                }
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
                this.$refs.optionMenu.style.left = e.clientX - 200 + "px";
                console.log(this.$refs.optionMenu.clientWidth);
                e.preventDefault();
            },
            test: function () {
                console.log(this.matchText);
            }
        }
    };
</script>

<style lang="less">
    html, body, div {
        margin: 0;
        padding: 0;
    }
    *{
        box-sizing: border-box;
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
        width: 100%;
        padding: 20px;

        #input_bar {
            display: flex;
            width: 100%;
            margin: 10px;
            border: 1px solid #bdbdbd;
            border-radius: 5px;
            align-items: center;
            height: 40px;
            font-size: 18px;

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
                font-size: 18px;
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

        #regex {
            display: flex;
            flex-grow: 1;
            width: 100%;
            height: 100%;
            margin: 9px;

            textarea {
                flex-basis: 50%;
                flex-grow: 1;
                margin-right: 5px;
                height: 100%;
                resize: none;
                border: 1px solid #bdbdbd;
                outline: none;
            }

            #result {
                flex-basis: 50%;
                flex-grow: 1;
                border: 1px solid #bdbdbd;
                height: 100%;
                margin-left: 5px;

                .matched {
                    background-color: #8d9fb0;
                }
            }
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
            width: 200px;

            div {
                padding: 10px 10px;
            }
        }
    }
</style>
