<template>
    <div id="app" class="container is-fluid">
        <div class="columns">
            <div class="column is-one-quarter">

            </div>
            <div class="column is-three-quarters">
                <div class="table-container">
                    <magic-table
                        :top-left-header="[['Table1']]"
                        :row-headers="rowHeaders"
                        :column-headers="columnHeaders"
                        :data="tableData"/>
                </div>
                <div class="table-container">
                    <magic-table
                        class="default"
                        :top-left-header="[['Table2']]"
                        :row-headers="rowHeaders"
                        :column-headers="columnHeaders"
                        :data="tableData"/>
                </div>
                <button 
                    @click="switchData">
                    Refresh
                </button>
            </div>
        </div>
    </div>
</template>

<script>
import MagicTable from './components/MagicTable'

export default {
    name: 'magic-table-demo',
    components: {
        MagicTable
    },
    data () {
        return {
            dataWidth: 20,
            dataHeight: 20,
            dataType: 'mult'
        }
    },
    computed: {
        rowHeaders () {
            const t = new Date().getTime()
            const data = []
            for (let i = 1; i <= this.dataHeight; i++) {
                data.push([i])
            }
            console.log('compute row headers', new Date().getTime() - t + 'ms')
            return data
        },
        columnHeaders () {
            const t = new Date().getTime()
            const data = []
            for (let i = 1; i <= this.dataWidth; i++) {
                data.push(i)
            }
            console.log('compute column headers', new Date().getTime() - t + 'ms')
            return [data]
        },
        tableData () {
            const t = new Date().getTime()
            const data = []
            for (let j = 1; j <= this.dataHeight; j++) {
                const row = []
                for (let i = 1; i <= this.dataWidth; i++) {
                    if (this.dataType === 'mult') {
                        if (i * j % 15 === 0) {
                            row.push('fizzbuzz')
                        } else if (i * j % 3 === 0) {
                            row.push('fizz')
                        } else if (i * j % 5 === 0) {
                            row.push('buzz')
                        } else {
                            row.push(i * j)
                        }
                    } else {
                        row.push(Math.pow(i, j))
                    }
                }
                data.push(row)
            }
            console.log('compute data', new Date().getTime() - t + 'ms')
            return data
        }
    },
    methods: {
        switchData () {
            if (this.dataType === 'mult') {
                this.dataType = 'exp'
            } else {
                this.dataType = 'mult'
            }
        }
    }
}
</script>

<style>
#app {
    font-family: 'Avenir', Helvetica, Arial, sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    text-align: center;
    color: #2c3e50;
}
</style>
