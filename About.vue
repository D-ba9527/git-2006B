<template>
    <div class="about">
        <div class="left">
            <h3>用户登录</h3>
            <p>
                <input type="text" v-model="username" />
                <span v-show="showSpan">用户名和必须6-18位</span>
            </p>
            <br />
            <button @click="succeed">{{login}}</button>
        </div>
        <div class="right">
            <div v-show="shadeShow" class="shade"></div>
            <div v-if="show1">
                <div v-show="listIndex==index" v-for="(item,index) in list" :key="item.id">
                    <h3>{{item.title}}</h3>
                    <p v-for="(eitem,eindex) in item.options" :key="eindex">
                        <input type="radio" :value="eindex" v-model="opt" />
                        <span>{{eitem}}</span>
                    </p>
                    <button @click="addOption(item)" :disabled="btnDis">{{add}}</button>
                    <span v-show="numShow">{{num}}后进入下一题</span>
                </div>
            </div>
            <div v-else>
                <span>
                    没有题目了
                    <button @click="start">再来一遍</button>
                </span>
            </div>
            <div>共有{{currNum}}/{{list.length}}道题 答对:{{trueNum}}/答错:{{falseNum}}</div>
        </div>
    </div>
</template>

<script>
    import datajson from '@/assets/data1.json'
    export default {
        data() {
            return {
                showUserName: true,
                showSpan: false,
                shadeShow: true,
                username: '',
                login: '登录',
                list: datajson.data,
                listIndex: 0,
                opt: -1,  // 单选框初始值
                num: 4,   //  倒计时初始值
                add: '提交',   // 提交按钮初始值
                trueNum: 0,  // 回答正确的数量
                falseNum: 0,   // 回答错误的数量
                currNum: 1,
                btnDis: false,
                numShow: false,
                show1: true
            }
        },
        mounted() {

        },
        // 事件方法
        methods: {
            // 登录成功
            succeed() {
                if (this.login == '登录') {

                    let or = /^[a-zA-Z0-9]{6,18}$/
                    if (!or.test(this.username)) {
                        this.showSpan = true

                    } else {
                        this.showSpan = false
                        this.shadeShow = false
                        this.username = ''
                        this.showUserName = false
                        this.login = '退出'
                    }
                } else {
                    this.showSpan = false,
                        this.shadeShow = true,
                        this.username = '',
                        this.login = '登录',
                        this.list = datajson.data,
                        this.listIndex = 0,
                        this.opt = -1,  // 单选框初始值
                        this.num = 4,   //  倒计时初始值
                        this.add = '提交',   // 提交按钮初始值
                        this.trueNum = 0,  // 回答正确的数量
                        this.falseNum = 0,   // 回答错误的数量
                        this.currNum = 1,
                        this.btnDis = false,
                        this.numShow = false,
                        this.show1 = true
                }

            },
            addOption(item) {
                if (item.right == this.opt) {
                    this.add = '回答正确'
                    this.trueNum++
                } else {
                    this.add = '回答错误'
                    this.falseNum++
                }
                console.log(this.opt);
                this.btnDis = true
                let timer = setInterval(() => {
                    this.num--
                    this.numShow = true
                    if (this.num == 0) {
                        clearInterval(timer)
                        this.num = 4
                        this.btnDis = false
                        clearInterval(timer)
                        this.add = '提交'
                        this.numShow = false
                        this.listIndex++
                        this.currNum++
                        this.opt = -1
                        if (this.listIndex == this.list.length) {
                            this.show1 = false
                            this.currNum = this.list.length
                        }
                    }
                }, 1000);
            },
            start() {
                this.listIndex = 0,
                    this.opt = -1,  // 单选框初始值
                    this.num = 4,   //  倒计时初始值
                    this.add = '提交',   // 提交按钮初始值
                    this.trueNum = 0,  // 回答正确的数量
                    this.falseNum = 0,   // 回答错误的数量
                    this.currNum = 1,
                    this.btnDis = false,
                    this.numShow = false,
                    this.show1 = true
            }
        },
        // 计算属性
        computed: {

        },
        // 事件监听
        watch: {

        },
    }
</script>

<style scoped  lang='scss'>
.about {
    width: 1200px;
    height: 400px;
    display: flex;
    margin: 50px auto;
    .left {
        width: 50%;
        height: 100%;
        border: 1px solid;
        span {
            color: red;
        }
    }
    .right {
        width: 49%;
        height: 100%;
        border: 1px solid;
        position: relative;
        .shade {
            position: absolute;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background-color: rgba(0, 0, 0, 0.8);
        }
    }
}
</style>
