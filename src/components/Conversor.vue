<template>
    <div class="conversor col-md-6 mb-4">
        <h2>Conversor de Moedas</h2>
        <div class="input-group firstValue">
            <input v-model="inputValue" @change="doConversion" type="number" 
            data-number-to-fixed="2" data-number-stepfactor="100" class="form-control currency" />
            <div class="input-group-append">
            <select class="form-select btn btn-outline-secondary dropdown-toggle" v-model="moedaChosen" @click="doConversion">
                <option>{{moedaA}}</option>
                <option>{{moedaB}}</option>
                <option>{{moedaC}}</option>
            </select>
            </div>
        </div>
        <ul class="list-group">
            <li class="list-group-item"><b-icon icon="calculator"></b-icon>
            {{moedaChosen}} 1 = {{moedaResult}} {{valueConvertedByOne}}</li>
            <li class="list-group-item"><b-icon icon="calculator"></b-icon>
            IOF(1,10%) = {{moedaChosen}} {{iof_value}}</li>
            <li class="list-group-item"><b-icon icon="calculator"></b-icon>
            Taxa administrativa = {{moedaChosen}} {{fx_value}}</li>
        </ul>
        <div class="input-group lastValue">
            <input v-model="valueConverted" @change="doConversion" type="number" 
            data-number-to-fixed="2" data-number-stepfactor="100" class="form-control currency" />
            <div class="input-group-append">
            <select class="form-select btn btn-outline-secondary dropdown-toggle" v-model="moedaResult" @click="doConversion">
                <option>{{moedaA}}</option>
                <option>{{moedaB}}</option>
                <option>{{moedaC}}</option>
            </select>
            </div>
        </div>
    </div>
</template>

<script>
export default {
    name: "Conversor",
    props: ["moedaA","moedaB","moedaC"],
    data() {
        return {
            moedaChosen: 'BRL',
            oldMoedaChosen: 'BRL',
            moedaResult: 'USD',
            oldMoedaResult: 'USD',
            inputValue: 0,
            valueConverted: 0,
            valueConvertedByOne: 0,
            iof_value: 0,
            fx_value: 0,
            fx_rate_usd_brl: 5.2164, 
            fx_rate_eur_brl: 6.3970, 
            fx_rate_eur_usd: 1.2206,
        }
    },
    methods: {
        doConversion() {
            if (this.moedaChosen == 'BRL') {
                if (this.moedaResult == 'USD') {
                    this.valueConverted = ((this.inputValue - this.iof_value - this.fx_value) / this.fx_rate_usd_brl).toFixed(2);
                    this.valueConvertedByOne = (1 / this.fx_rate_usd_brl).toFixed(3);
                }
                if (this.moedaResult == 'EUR') {
                    this.valueConverted = ((this.inputValue - this.iof_value - this.fx_value) / this.fx_rate_eur_brl).toFixed(2);
                    this.valueConvertedByOne = (1 / this.fx_rate_eur_brl).toFixed(3);
                }
            } else if (this.moedaChosen == 'USD') {
                if (this.moedaResult == 'BRL') {
                    this.valueConverted = ((this.inputValue - this.iof_value - this.fx_value) * this.fx_rate_usd_brl).toFixed(2);
                    this.valueConvertedByOne = (1 * this.fx_rate_usd_brl).toFixed(3);
                }
                if (this.moedaResult == 'EUR') {
                    this.valueConverted = ((this.inputValue - this.iof_value - this.fx_value) / this.fx_rate_eur_usd).toFixed(2);
                    this.valueConvertedByOne = (1 / this.fx_rate_eur_usd).toFixed(3);
                }
            } else {
                if (this.moedaResult == 'BRL') {
                    this.valueConverted = ((this.inputValue - this.iof_value - this.fx_value) * this.fx_rate_eur_brl).toFixed(2);
                    this.valueConvertedByOne = (1 * this.fx_rate_eur_brl).toFixed(3);
                }
                if (this.moedaResult == 'USD') {
                    this.valueConverted = ((this.inputValue - this.iof_value - this.fx_value) * this.fx_rate_eur_usd).toFixed(2);
                    this.valueConvertedByOne = (1 * this.fx_rate_eur_usd).toFixed(3);
                }
            }

            this.iof_value = (this.inputValue * 0.011).toFixed(2);
            this.fx_value = (this.inputValue * 0.01).toFixed(2);
        },
    },
    watch: {
        moedaChosen(newValue, oldValue) {
            this.oldMoedaChosen = oldValue;
            if (this.moedaChosen == this.moedaResult) {
                this.moedaResult = this.oldMoedaChosen;
                this.doConversion();
            } else {
                this.oldMoedaChosen = this.moedaChosen;
            }
        },
        moedaResult(newValue, oldValue) {
            this.oldMoedaResult = oldValue;
            if (this.moedaResult == this.moedaChosen) {
                this.moedaChosen = this.oldMoedaResult;
                this.doConversion();
            } else {
                this.oldMoedaResult = this.moedaResult;
            }
        }
    }
    
}
</script>

<style scoped>
.firstValue, .lastValue {
    margin-top: 5%;
}
.firstValue {
    margin-bottom: 5%;
}
.conversor {
    margin-left: 25%;
}

h2 {
    text-align: center;
}

input.currency {
    text-align: left;
    padding-right: 15px;
}
</style>