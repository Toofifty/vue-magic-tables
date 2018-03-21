<template>
    <div
        class="magic-table"
        :key="dataChecksum"
        :data-key="dataChecksum"
        :style="{
            '--cell-width': maxCellWidth + 'px',
            '--cell-height': maxCellHeight + 'px',
            '--header-height': maxHeaderHeight + 'px'
        }">
        <div class="magic-table__struct__left">
            <top-left
                ref="topLeft"
                :data="topLeftHeader"
                :header-height="maxHeaderHeight"
                :css-vars="cssVars"/>
            <row-header
                ref="rowHeader"
                :data="rowHeaders"
                :offset-y="offsetY"
                :cell-height="maxCellHeight"
                :css-vars="cssVars"/>
        </div>
        <div class="magic-table__struct__right">
            <column-header
                ref="columnHeader"
                :data="columnHeaders"
                :offset-x="offsetX"
                :cell-width="maxCellWidth"
                :header-height="maxHeaderHeight"
                :css-vars="cssVars"/>
            <table-data
                ref="dataTable"
                :data="data"
                :offset-x="offsetX"
                :offset-y="offsetY"
                :cell-width="maxCellWidth"
                :cell-height="maxCellHeight"
                :pannable="pannable"
                :css-vars="cssVars"
                @update-offset="updateOffset"/>
        </div>
    </div>
</template>

<script>
import TableData from './TableData'
import TopLeft from './TopLeft'
import ColumnHeader from './ColumnHeader'
import RowHeader from './RowHeader'

export default {
    name: 'magic-table',
    components: {
        TableData,
        TopLeft,
        ColumnHeader,
        RowHeader
    },
    props: {
        rowHeaders: {
            type: Array,
            default: () => [
                ['row-header-1'],
                ['row-header-2'],
                ['row-header-3'],
                ['row-header-4'],
                ['row-header-5'],
                ['row-header-6'],
                ['row-header-7'],
                ['row-header-8'],
                ['row-header-9'],
                ['row-header-10']
            ]
        },
        columnHeaders: {
            type: Array,
            default: () => [[
                'column-header-1',
                'column-header-2',
                'column-header-3',
                'column-header-4',
                'column-header-5',
                'column-header-6',
                'column-header-7',
                'column-header-8',
                'column-header-9',
                'column-header-10'
            ]]
        },
        topLeftHeader: {
            type: Array,
            default: () => [[]]
        },
        data: {
            type: Array,
            default: () => [
                [
                    'row-1 column-1',
                    'row-1 column-2',
                    'row-1 column-3',
                    'row-1 column-4',
                    'row-1 column-5',
                    'row-1 column-6',
                    'row-1 column-7',
                    'row-1 column-8',
                    'row-1 column-9',
                    'row-1 column-10'
                ],
                [
                    'row-2 column-1',
                    'row-2 column-2',
                    'row-2 column-3',
                    'row-2 column-4',
                    'row-2 column-5',
                    'row-2 column-6',
                    'row-2 column-7',
                    'row-2 column-8',
                    'row-2 column-9',
                    'row-2 column-10'
                ],
                [
                    'row-3 column-1',
                    'row-3 column-2',
                    'row-3 column-3',
                    'row-3 column-4',
                    'row-3 column-5',
                    'row-3 column-6',
                    'row-3 column-7',
                    'row-3 column-8',
                    'row-3 column-9',
                    'row-3 column-10'
                ],
                [
                    'row-4 column-1',
                    'row-4 column-2',
                    'row-4 column-3',
                    'row-4 column-4',
                    'row-4 column-5',
                    'row-4 column-6',
                    'row-4 column-7',
                    'row-4 column-8',
                    'row-4 column-9',
                    'row-4 column-10'
                ],
                [
                    'row-5 column-1',
                    'row-5 column-2',
                    'row-5 column-3',
                    'row-5 column-4',
                    'row-5 column-5',
                    'row-5 column-6',
                    'row-5 column-7',
                    'row-5 column-8',
                    'row-5 column-9',
                    'row-5 column-10'
                ],
                [
                    'row-6 column-1',
                    'row-6 column-2',
                    'row-6 column-3',
                    'row-6 column-4',
                    'row-6 column-5',
                    'row-6 column-6',
                    'row-6 column-7',
                    'row-6 column-8',
                    'row-6 column-9',
                    'row-6 column-10'
                ],
                [
                    'row-7 column-1',
                    'row-7 column-2',
                    'row-7 column-3',
                    'row-7 column-4',
                    'row-7 column-5',
                    'row-7 column-6',
                    'row-7 column-7',
                    'row-7 column-8',
                    'row-7 column-9',
                    'row-7 column-10'
                ],
                [
                    'row-8 column-1',
                    'row-8 column-2',
                    'row-8 column-3',
                    'row-8 column-4',
                    'row-8 column-5',
                    'row-8 column-6',
                    'row-8 column-7',
                    'row-8 column-8',
                    'row-8 column-9',
                    'row-8 column-10'
                ],
                [
                    'row-9 column-1',
                    'row-9 column-2',
                    'row-9 column-3',
                    'row-9 column-4',
                    'row-9 column-5',
                    'row-9 column-6',
                    'row-9 column-7',
                    'row-9 column-8',
                    'row-9 column-9',
                    'row-9 column-10'
                ],
                [
                    'row-10 column-1',
                    'row-10 column-2',
                    'row-10 column-3',
                    'row-10 column-4',
                    'row-10 column-5',
                    'row-10 column-6',
                    'row-10 column-7',
                    'row-10 column-8',
                    'row-10 column-9',
                    'row-10 column-10'
                ],
            ]
        },
        pannable: {
            type: Boolean,
            default: true
        }
    },
    data () {
        return {
            offsetX: 0,
            offsetY: 0,
            maxCellWidth: 0,
            maxCellHeight: 0,
            maxHeaderHeight: 0,
            cssVars: false
        }
    },
    computed: {
        dataChecksum () {
            const str = JSON.stringify(this.data)
                + JSON.stringify(this.rowHeaders)
                + JSON.stringify(this.columnHeaders)
            let check = 0x12345678
            const len = str.length
            for (let i = 0; i < len; i++) {
                check += (str.charCodeAt(i) * (i + 1))
            }
            return (check & 0xffffffff).toString(16)
        }
    },
    created () {
        this.cssVars = window.CSS && CSS.supports('color', 'var(--primary)')
    },
    mounted () {
        this.refreshCellSizes()
        this.$watch('dataChecksum', () => {
            this.maxCellWidth = 0
            this.maxCellHeight = 0
            this.$nextTick(() => {
                this.refreshCellSizes()
            })
        })
    },
    methods: {
        updateOffset ({ x, y }) {
            if (x >= 0 && x <= this.$refs.dataTable.getMaxScrollX()) {
                this.offsetX = x
            }
            if (y >= 0 && y <= this.$refs.dataTable.getMaxScrollY()) {
                this.offsetY = y
            }
        },
        refreshCellSizes () {
            this.maxHeaderHeight = Math.max(
                this.$refs.topLeft.getTotalHeight(),
                this.$refs.columnHeader.getTotalHeight()
            )
            this.maxCellWidth = Math.max(
                this.$refs.dataTable.getMaxCellWidth(),
                this.$refs.columnHeader.getMaxCellWidth()
            )
            this.maxCellHeight = Math.max(
                this.$refs.dataTable.getMaxCellHeight(),
                this.$refs.rowHeader.getMaxCellHeight()
            )
        }
    }
}
</script>