<template>
    <div class="container">
        <form @submit.prevent.stop="calculate">
            <h1>{{ result }}</h1>
            <div class="fields">
                <input type="number" placeholder="Valor x" v-model="x"/>
                <input type="number" placeholder="Valor y" v-model="y"/>
                <select v-model="option">
                    <option value="som">Soma</option>
                    <option value="sub">Subtração</option>
                    <option value="mult">Multiplicação</option>
                    <option value="div">Divisão</option>
                </select>
            </div>
            <input type="submit"/>
        </form>
        <div class="history">
            <H1>History</H1>
            <button id="clearHistory" @click="clearHistory">Clear</button>
            <table>
                <thead>
                    <tr>
                        <th>#</th>
                        <th>Operation</th>
                        <th>Value</th>
                        <th>Result</th>
                        <th>Time</th>
                    </tr>
                </thead>
                <tbody>
                    <tr v-for="calc in history" :key="calc">
                        <td>
                            <div class="calcId" :class="{
                                'calc-sum': calc.operation == 'Soma',
                                'calc-sub': calc.operation == 'Subtração',
                                'calc-mult': calc.operation == 'Multiplicação',
                                'calc-div': calc.operation == 'Divisão'
                            }">

                            </div>
                        </td>
                        <td>{{ calc.operation }}</td>
                        <td>{{ calc.value }}</td>
                        <td>{{ calc.result }}</td>
                        <td>{{ calc.time }}</td>
                    </tr>
                </tbody>
            </table>
        </div>
    </div>
</template>

<script lang="ts">
    import Vue from "vue"

    interface IHistory {
        operation: string;
        value: string;
        result: number;
        time: string;
    }

    interface IData {
        x: number;
        y: number;
        option: string;
        result: number | string;
        history: IHistory[]
    }

    export default Vue.extend({
        data(): IData {
            return{
                x: 0,
                y: 0,
                option: " ",
                result: "Aguardando resultado... ",
                history: []
            }
        },
        methods: {
            calculate(){
                const xValue = Number(this.x)
                const yValue = Number(this.y)

                if(this.x == 0 && this.y == 0)
                    return this.result = "Você precisa inserir algum valor válido"

                if(this.option == " "){
                    return this.result = "Opção Invalida"
                }

                if(this.option == "som"){
                    let calc = xValue + yValue
                    this.history.push({
                        operation: "Soma",
                        result: calc,
                        value: `${xValue} + ${yValue}`,
                        time: this.dateTime()
                    })
                    return this.result = calc
                }
                
                if(this.option == "sub"){
                    let calc = xValue - yValue
                    this.history.push({
                        operation: "Subtração",
                        result: calc,
                        value: `${xValue} - ${yValue}`,
                        time: this.dateTime()
                    })
                    return this.result = calc
                }

                if(this.option == "mult"){
                    let calc = xValue * yValue
                    this.history.push({
                        operation: "Multiplicação",
                        result: calc,
                        value: `${xValue} * ${yValue}`,
                        time: this.dateTime()
                    })
                    return this.result = calc
                }

                if(this.option == "div"){
                    let calc = xValue / yValue
                    this.history.push({
                        operation: "Divisão",
                        result: calc,
                        value: `${xValue} / ${yValue}`,
                        time: this.dateTime()
                    })
                    return this.result = calc
                }
            },
            dateTime(): string {
                let time = new Date()
                return `${time.getHours()}:${time.getMinutes()}:${time.getSeconds()}`
            },
            clearHistory(){
                this.history = []
            }
        },
    })
</script>

<style scoped>
    .container{
        display: flex;
        align-items: center;
        flex-direction: row-reverse;
        justify-content: space-evenly;
        height: 100vh;
        gap: 15px;
    }
    .history{
        display: flex;
        align-items: center;
        flex-direction: column;
        gap: 15px;
        height: 70%;
        overflow-y: scroll;
    }
    .history h1{
        color: grey;
        font-size: 30px;
    }
    form {
        display: flex;
        flex-direction: column;
        gap: 15px;
    }
    form h1{
        color: grey;
        font-size: 30px;
        text-align: center;
    }
    .fields{
        display: flex;
        gap: 10px;
    }
    .fields input::-webkit-inner-spin-button{
        display: none;
    }
    .fields input, .fields select{
        padding: 10px;
        border: none;
        border-bottom: 2px solid grey;
    }
    table {
        border: 2px solid gray;
        border-radius: 3px;
        box-shadow: 0px 0px 10px #212121;
        
    }
    tr, td, th {
        padding: 10px;
    }
    input[type = "submit"], #clearHistory{
        padding: 10px;
        border-radius: 50px;
        background-color: purple;
        color: white;
        font-weight: bold;
        text-transform: uppercase;
        width: 200px;
        align-self: center;
    }
    .calcId{
        width: 30px;
        height: 30px;
        border-radius: 50px;
        background-color: chocolate;
    }
    .history::-webkit-scrollbar{
        width: 0px;
    }
    .calc-sum{
        background-color: yellow;
    }
    .calc-sub{
        background-color: darkmagenta;
    }
    .calc-div{
        background-color: red;
    }
    .calc-mult{
        background-color: blueviolet;
    }

</style>
