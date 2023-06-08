<template>
    <div>
        <button @click="changeView(1)">顯示單向綁定</button>
        <button @click="changeView(2)">顯示雙向綁定</button>
        <button @click="changeView(3)">沒畫面</button>
        <div class="container-1" v-if="view === 1">
            <p>單向綁定</p>
            <p>{{data}}</p>
        </div>

        <div class="container-2" v-else-if="view === 2">
            <p>雙向綁定</p>
            <input type="text" v-model="text"/>
            <p>{{text}}</p>
        </div>

        <div v-else>沒畫面</div>
        <div v-for="(value, key, index) in dataList" :key="value">
                <p v-if="key !== 'key3'"> {{index+1}}. {{key}}: {{value}} </p>
        </div>
        <div class="text-cls" :key="test">{{test}}</div>
        <div>
            <input type="text" v-model="text"/>
            <br>
            <input type="number" v-model="num"/>
            <p>{{numFormat}}</p>
        </div>
        <div>
            <input type="text" v-model="input.levels.text"/>
            <input type="text" v-model="input.label"/>
        </div>
    </div>
</template>

<script setup>
    import {ref, computed, reactive, watch, watchEffect} from 'vue'
    const data = 'Hello Vue!!!'
    const text = ref('123')
    const test = ref('test')
    const view = ref(1)
    const num = ref(0)
    const input = reactive({
        levels: {
            text: '0'
        },
        label: '123'
    })
    const changeView = (index) => {
        view.value = index
    }
    const numFormat = computed(() => {
        if (Number.isNaN(num.value*100)) return num.value
        return `${num.value*100}%`
    })
    const dataList = {
        key1: 'a',
        key2: 'b',
        key3: 'c',
        key4: 'd',
    }

    watch(
        [() => input.levels.text, () => input.label],
        (newData, oldData) => {
            console.log(newData, oldData)
        }
    )

    const stop = watchEffect(() => {
        console.log(input.levels.text)
        if (input.levels.text === 'stop') stop()
    })

    setTimeout(() => {
        test.value = 'apple!'
    }, 2000)
</script>

<style>
    .container-1 {
        border: 2px red solid;
    }
    .container-2 {
        border: 2px skyblue solid;
    }
    .text-cls {
        font-size: 40px;
        animation: openIn 1s ease-in-out;
    }

    @keyframes openIn {
        0% {
            opacity: 0;
            transform: translateY(20px);
        }
        100% {
            opacity: 1;
            transform: translateY(0);
        }
    }
</style>
