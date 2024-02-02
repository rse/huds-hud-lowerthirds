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
    <control v-if="mode === 'control'"
        v-bind:canvasBg="config.control.canvasBg"
        v-bind:nameFn="config.control.nameFn"
        v-bind:nameBg="config.control.nameBg"
        v-bind:nameFg="config.control.nameFg"
        v-bind:textFn="config.control.textFn"
        v-bind:textBg="config.control.textBg"
        v-bind:textFg="config.control.textFg"
    ></control>
    <render v-if="mode === 'render'"
        v-bind:textFn="config.render.textFn"
        v-bind:textFs="config.render.textFs"
        v-bind:textBg="config.render.textBg"
        v-bind:textFg="config.render.textFg"
    ></render>
</template>

<script>
export default {
    name: "index",
    data: () => ({
        config: huds.config(),
        mode: typeof huds.options.mode === "string" ? huds.options.mode : "render"
    }),
    components: {
        "control": Vue.loadComponent("control.vue"),
        "render":  Vue.loadComponent("render.vue")
    }
}
</script>

