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
    <div v-bind:style="style" class="render">
        <div v-for="field in fields" class="field"
            v-bind:style="{ left: field.x, top: field.y }">
            {{ field.text }}
        </div>
    </div>
</template>

<style lang="less" scoped>
.render {
    position:         relative;
}
.render .field {
    position:         absolute;
    top:              0;
    left:             0;
    background-color: var(--textBg);
    color:            var(--textFg);
    font-family:      var(--textFn);
    font-size:        var(--textFs);
}
</style>

<script>
export default {
    name: "render",
    props: {
        textFn:     { type: String, default: "" },
        textFs:     { type: String, default: "" },
        textBg:     { type: String, default: "" },
        textFg:     { type: String, default: "" }
    },
    data: () => ({
        fields: []
    }),
    computed: {
        style: HUDS.vueprop2cssvar()
    },
    created () {
        if (!(typeof huds.options.fields === "string" && huds.options.fields)) {
            alert("missing field definition")
            return
        }
        for (const def of huds.options.fields.split("+")) {
            const m =  def.match(/^(.+):([+.]?\d+):([+-]?\d+)$/)
            this.fields.push({
                name: m[1],
                x:    parseInt(m[2]) + "px",
                y:    parseInt(m[3]) + "px",
                text: ""
            })
        }
    },
    mounted () {
        huds.bind("lowerthirds.update.response", (event, data) => {
            const field = this.fields.find((field) => field.name === data.name)
            if (field)
                field.text = data.text
        })
        huds.send("lowerthirds.update.request")
    },
    methods: {
    }
}
</script>

