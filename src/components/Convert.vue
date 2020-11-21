<template>
    <div class="curse">
<h2>Converter</h2>
		<span>Input val</span><input type="number" v-model = "currency"> <br>
		<span>from</span>
		<select  v-model = "Val">
			<option v-for="item in conver" :value="item.buy"  v-bind:key="item.ccy">{{ item.ccy }}</option>
		</select>
		<span>in</span>
		<select v-model = "ValConvert">
			<option v-for="item in conver" :value="item.buy"  v-bind:key="item.ccy">{{ item.ccy }}</option>
		</select>
		<button v-on:click="calculate()">Convert</button>
		<h3 v-if="converted">{{ res }}</h3>
    </div>
    
</template>

<script>

import Vue from 'vue'
import axios from 'axios'
import VueAxios from 'vue-axios'


export default {
    data: function(){
        return {
           currency:0,
        conver:[],
        res: "",
        converted: true,
        Val: "",
        ValConvert: "",
        };
    },
    mounted: function(){
        axios.get("https://api.privatbank.ua/p24api/pubinfo?json&exchange&coursid=5").then((response) =>{
            console.log(response.data);
            this.conver = response.data;
        })
    },
    methods: {
        calculate: function(){
            this.res = this.Val / this.ValConvert * this.currency
        }
    }
}
</script>
