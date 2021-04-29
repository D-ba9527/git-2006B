<template>
    <div>
        <div class="top">
            <p>
                <button @click="stop">{{stopState}}</button>
                <br />所在城市：
                <select v-model="optionVal" :disabled="disabled">
                    <option v-for="item in data" :key="item.id" :value="item.name">{{item.name}}</option>
                </select>
            </p>
            <p>
                姓名：
                <input type="text" v-model="userName" :disabled="disabled" />
            </p>
            <p>
                年龄：
                <input type="text" v-model="age" :disabled="disabled" />
            </p>
            <p>
                <button @click="add" :disabled="disabled">{{addVal}}</button>
                <button @click="a" :disabled="disabled">城市{{csSort}}</button>
                <button @click="b" :disabled="disabled">年龄{{ageSort}}</button>
                <br />
                <input type="text" placeholder="请输入城市" v-model="cVal" :disabled="disabled" />
                <button @click="c" :disabled="disabled">城市搜索</button>
                <br />
                <input type="text" placeholder="请输入姓名" v-model="dVal" :disabled="disabled" />
                <button @click="d" :disabled="disabled">姓名搜索</button>
                <br />
                <input type="text" v-model="eVal1" :disabled="disabled" />-
                <input type="text" v-model="eVal2" :disabled="disabled" />
                <button @click="e" :disabled="disabled">年龄搜索</button>
            </p>
        </div>
        <div class="content">
            <table width="800" border="1" cellspacing="0">
                <thead>
                    <tr>
                        <th></th>
                        <th>ID</th>
                        <th>姓名</th>
                        <th>年龄</th>
                        <th>城市</th>
                        <th>操作</th>
                    </tr>
                </thead>
                <tbody align="center">
                    <tr
                        v-for="(item,index) in list"
                        :key="item.id"
                        :class="index% 2== 0?'active': ''"
                    >
                        <td>
                            <input type="checkbox" :disabled="disabled" v-model="item.check" />
                        </td>
                        <td>{{item.id}}</td>
                        <td>{{item._userName}}</td>
                        <td>{{item._age}}</td>
                        <td>{{item._optionVal}}</td>
                        <td>
                            <button :disabled="disabled" @click="redact(index)">编辑</button>
                            <button @click="del(index)" :disabled="disabled">删除</button>
                        </td>
                    </tr>
                </tbody>
            </table>
            <button @click="select" :disabled="disabled">删除选中</button>
        </div>
        <test2></test2>
    </div>
</template>

<script>
    import datajson from '@/assets/data.json'
    import test2 from '@/components/test2.vue'
    export default {
        data() {
            return {
                data: datajson.data,
                optionVal: '',
                userName: '',
                age: '',
                addVal: '提交',
                num: 1,
                list: [],
                stopState: '锁定',
                disabled: false,
                time: 4,
                listIndex: '',  // 编辑需要的下标
                cVal: '',
                dVal: '',
                eVal1: '',
                eVal2: '',
                ageSort: '默认',
                listCopy: [],
                csSort: '默认'
            }
        },
        components: {
            test2
        },
        mounted() {

        },
        // 事件方法
        methods: {
            // 城市搜索
            a() {
                if (this.csSort == '默认') {
                    this.listCopy = JSON.parse(JSON.stringify(this.list))
                    this.csSort = '正序'
                    this.list.sort((a, b) => a._optionVal.localeCompare(b._optionVal))
                } else if (this.csSort == '正序') {
                    this.csSort = '倒序'
                    this.list.sort((a, b) => b._optionVal.localeCompare(a._optionVal))
                } else if (this.csSort == '倒序') {
                    this.csSort = '默认'
                    this.list = this.listCopy
                }
            },
            // 年龄排序
            b() {
                if (this.ageSort === '默认') {
                    this.listCopy = JSON.parse(JSON.stringify(this.list))
                    this.ageSort = '大到小'
                    this.list.sort((a, b) => {
                        return b._age - a._age
                    })
                } else if (this.ageSort === '大到小') {
                    this.ageSort = '小到大'
                    this.list.sort((a, b) => {
                        return a._age - b._age
                    })
                } else if (this.ageSort === '小到大') {
                    this.ageSort = '默认'
                    this.list = this.listCopy
                }
            },
            // 城市搜索
            c() {
                this.listCopy = JSON.parse(JSON.stringify(this.list))
                if (this.cVal) {
                    let arr = this.list.filter(item => {
                        return item._optionVal.includes(this.cVal)
                    })
                    this.list = arr
                } else {
                    this.list = this.listCopy
                }
            },
            // 姓名搜索
            d() {
                this.listCopy = JSON.parse(JSON.stringify(this.list))
                if (this.dVal) {
                    let arr = this.list.filter(item => {
                        return item._userName.includes(this.dVal)
                    })
                    this.list = arr
                } else {
                    this.list = this.listCopy
                }
            },
            // 年龄区间搜索
            e() {
                this.listCopy = JSON.parse(JSON.stringify(this.list))
                if (this.eVal1 && this.eVal2) {
                    let arr = this.list.filter(item => {
                        if (item._age >= this.eVal1 && item._age <= this.eVal2) {
                            return item
                        }
                    })
                    this.list = arr
                } else {
                    this.list = this.listCopy
                }
            },
            // 添加方法
            add() {
                let obj = {
                    id: this.num,
                    _userName: this.userName,
                    _age: this.age,
                    _optionVal: this.optionVal,
                    check: false
                }
                if (this.userName == '' && this.age == '' && this.optionVal == '') {
                    alert('添加不能为空')
                } else {
                    if (this.addVal == '提交') {
                        this.num++
                        this.list.push(obj)
                    } else {
                        this.addVal = '提交'
                        obj.id = this.list[this.listIndex].id
                        this.list.splice(this.listIndex, 1, obj)
                    }
                }
                this.userName = ''
                this.age = ''
                this.optionVal = ''
            },
            redact(index) {
                let data = this.list[index]
                this.userName = data._userName
                this.age = data._age
                this.optionVal = data._optionVal
                this.addVal = '确认修改'
                this.listIndex = index
                console.log(this.listIndex);
            },
            // 禁用方法
            stop() {
                if (this.stopState == '锁定') {

                    this.stopState = '解锁'
                    this.disabled = true
                } else {
                    let timer = setInterval(() => {
                        this.time--
                        this.stopState = this.time + '秒后解锁'
                        if (this.time == 0) {
                            clearInterval(timer)
                            this.stopState = '解除禁用'
                            this.time = 4
                            this.disabled = false
                        }
                    }, 1000);
                }
            },
            // 删除选中
            select() {
                let _list = this.list
                for (let i = _list.length - 1; i >= 0; i--) {
                    if (_list[i].check) {
                        this.list.splice(i, 1)
                    }
                }
            },
            // 删除
            del(index) {
                this.list.splice(index, 1)
            },
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
.active {
    background-color: rgb(206, 206, 206);
}
</style>
