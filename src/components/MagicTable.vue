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
            default: () => [[]]
        },
        columnHeaders: {
            type: Array,
            default: () => [[]]
        },
        topLeftHeader: {
            type: Array,
            default: () => [[]]
        },
        data: {
            type: Array,
            default: () => [[]]
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