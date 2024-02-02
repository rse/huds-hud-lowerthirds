<!--
/*
**  HUDS ~~ Head-Up-Display Server (HUDS)
**  Copyright (c) 2020-2024 Dr. Ralf S. Engelschall <rse@engelschall.com>
**
**  Permission is hereby granted, free of charge, to any person obtaining
**  a copy of this software and associated documentation files (the
**  "Software"), to deal in the Software without restriction, including
**  without limitation the rights to use, copy, modify, merge, publish,
**  distribute, sublicense, and/or sell copies of the Software, and to
**  permit persons to whom the Software is furnished to do so, subject to
**  the following conditions:
**
**  The above copyright notice and this permission notice shall be included
**  in all copies or substantial portions of the Software.
**
**  THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND,
**  EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF
**  MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT.
**  IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY
**  CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT,
**  TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE
**  SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.
*/
-->

<template>
    <div v-bind:style="style" class="control">
        <div class="field">
            <div class="name"><input v-model="fields[0].name" v-on:keyup="mod(0)"/></div>
            <div class="text"><input v-model="fields[0].text" v-on:keyup="mod(0)"/></div>
        </div>
        <div class="field">
            <div class="name"><input v-model="fields[1].name" v-on:keyup="mod(1)"/></div>
            <div class="text"><input v-model="fields[1].text" v-on:keyup="mod(1)"/></div>
        </div>
        <div class="field">
            <div class="name"><input v-model="fields[2].name" v-on:change="mod(2)"/></div>
            <div class="text"><input v-model="fields[2].text" v-on:change="mod(2)"/></div>
        </div>
        <div class="field">
            <div class="name"><input v-model="fields[3].name" v-on:change="mod(3)"/></div>
            <div class="text"><input v-model="fields[3].text" v-on:change="mod(3)"/></div>
        </div>
        <div class="field">
            <div class="name"><input v-model="fields[4].name" v-on:change="mod(4)"/></div>
            <div class="text"><input v-model="fields[4].text" v-on:change="mod(4)"/></div>
        </div>
        <div class="field">
            <div class="name"><input v-model="fields[5].name" v-on:change="mod(5)"/></div>
            <div class="text"><input v-model="fields[5].text" v-on:change="mod(5)"/></div>
        </div>
        <div class="field">
            <div class="name"><input v-model="fields[6].name" v-on:change="mod(6)"/></div>
            <div class="text"><input v-model="fields[6].text" v-on:change="mod(6)"/></div>
        </div>
        <div class="field">
            <div class="name"><input v-model="fields[7].name" v-on:change="mod(7)"/></div>
            <div class="text"><input v-model="fields[7].text" v-on:change="mod(7)"/></div>
        </div>
    </div>
</template>

<style lang="less" scoped>
.control {
    position:         relative;
    background-color: var(--canvasBg);
    width:            100vw;
    height:           100vh;
    padding:          20px;
}
.control .field {
    display:          flex;
    flex-direction:   row;
    justify-items:    center;
    align-items:      center;
    margin-bottom:    8px;
}
.control .field .name input {
    font-family:      var(--nameFn), sans-serif;
    font-size:        16pt;
    width:            200px;
    outline:          none;
    background-color: var(--nameBg);
    color:            var(--nameFg);
    border:           0;
    border-top-left-radius:    8px;
    border-bottom-left-radius: 8px;
    padding:          4px 10px 4px 10px;
    height:           30px;
}
.control .field .text input {
    font-family:      var(--textFn), sans-serif;
    font-family:      sans-serif;
    font-size:        16pt;
    width:            400px;
    outline:          none;
    background-color: var(--textBg);
    color:            var(--textFg);
    border:           0;
    border-top-right-radius:    8px;
    border-bottom-right-radius: 8px;
    padding:          4px 10px 4px 10px;
    height:           30px;
}
</style>

<script>
const timer = {}
export default {
    name: "control",
    props: {
        canvasBg:   { type: String, default: "" },
        nameFn:     { type: String, default: "" },
        nameBg:     { type: String, default: "" },
        nameFg:     { type: String, default: "" },
        textFn:     { type: String, default: "" },
        textBg:     { type: String, default: "" },
        textFg:     { type: String, default: "" }
    },
    data: () => ({
        fields: [
            { slot: 0, name: "scene-1", text: "" },
            { slot: 1, name: "scene-2", text: "" },
            { slot: 2, name: "scene-3", text: "" },
            { slot: 3, name: "scene-4", text: "" },
            { slot: 4, name: "scene-5", text: "" },
            { slot: 5, name: "scene-6", text: "" },
            { slot: 6, name: "scene-7", text: "" },
            { slot: 7, name: "scene-8", text: "" }
        ]
    }),
    computed: {
        style: HUDS.vueprop2cssvar()
    },
    mounted () {
        this.update()
        huds.bind("lowerthirds.update.request", (event, data) => {
            this.update()
        })
    },
    methods: {
        update (slot) {
            if (typeof slot === "number")
                huds.send("lowerthirds.update.response", this.fields[slot])
            else
                for (let i = 0; i < this.fields.length; i++)
                    huds.send("lowerthirds.update.response", this.fields[i])
        },
        mod (slot) {
            if (timer[slot])
                clearTimeout(timer[slot])
            timer[slot] = setTimeout(() => {
                delete timer[slot]
                this.update(slot)
            }, 1000)
        }
    }
}
</script>

