<template>
    <div
        class="magic-table__data__container"
        :style="{
            cursor: pannable 
                ? isPanning
                    ? ['-webkit-grabbing', 'grabbing']
                    : ['-webkit-grab', 'grab']
                : null
        }"
        @scroll="updateByScroll"
        @mousedown="beginPan">
        <table
            class="magic-table__data__table"
            ref="table"
            :class="{
                'no-select': pannable    
            }"
            v-once>
            <tbody
                class="magic-table__data__body">
                <tr
                    class="magic-table__data__row"
                    ref="row"
                    v-for="(row, rowIndex) in data"
                    :key="rowIndex">
                    <td
                        class="magic-table__data__cell"
                        :ref="'cell' + rowIndex"
                        v-for="(cell, cellIndex) in row"
                        :key="cellIndex"
                        :style="!cssVars ? {
                                minWidth: cellWidth + 'px',
                                height: cellHeight + 'px'
                            } : null">
                        {{ cell }}
                    </td>
                </tr>
            </tbody>
        </table>
    </div>
</template>

<script>
export default {
    name: 'table-data',
    props: {
        data: {
            type: Array,
            default: []
        },
        offsetX: {
            type: Number,
            default: 0
        },
        offsetY: {
            type: Number,
            default: 0
        },
        spacingTop: {
            type: Number,
            default: 0
        },
        spacingLeft: {
            type: Number,
            default: 0
        },
        cellWidth: {
            type: Number,
            default: 0
        },
        cellHeight: {
            type: Number,
            default: 0
        },
        cssVars: {
            type: Boolean,
            default: false
        },
        pannable: {
            type: Boolean,
            default: true
        }
    },
    data () {
        return {
            isPanning: false,
            supportsCSSVars: false
        }
    },
    mounted () {
        this.supportsCSSVars = window.CSS && CSS.supports('color', 'var(--primary)')
        this.$watch('offsetX', (offsetX) => {
            this.$el.scrollLeft = offsetX
        })
        this.$watch('offsetY', (offsetY) => {
            this.$el.scrollTop = offsetY
        })
    },
    methods: {
        updateOffset (x, y) {
            this.$emit('update-offset', { x, y })
        },
        updateByScroll () {
            this.updateOffset(
                this.$el.scrollLeft,
                this.$el.scrollTop
            )
        },
        updateByPan (event) {
            this.updateOffset(
                this.offsetX - event.movementX,
                this.offsetY - event.movementY
            )
        },
        beginPan () {
            if (!this.isPanning) {
                this.isPanning = true
                document.addEventListener('mousemove', this.updateByPan)
                document.addEventListener('mouseup', this.endPan)
            }
        },
        endPan () {
            this.isPanning = false
            document.removeEventListener('mousemove', this.updateByPan)
        },
        getMaxCellWidth () {
            return Math.max(...(this.$refs.cell0.map(v => v.clientWidth)))
        },
        getMaxCellHeight () {
            return Math.max(...(this.$refs.row.map(v => v.clientHeight)))
        },
        getMaxScrollX () {
            return this.$refs.table.clientWidth - this.$el.clientWidth
        },
        getMaxScrollY () {
            return this.$refs.table.clientHeight - this.$el.clientHeight
        }
    }
}
</script>
