<template>
    <div
        class="magic-table__header-row__container">
        <table
            class="magic-table__header-row__table">
            <thead
                class="magic-table__header-row__head">
                <tr
                    class="magic-table__header-row__row"
                    :key="rowIndex"
                    v-for="(row, rowIndex) in data">
                    <th
                        class="magic-table__header-row__cell"
                        :class="cell.class ? cell.class : null"
                        :ref="'cell' + rowIndex"
                        :key="cellIndex"
                        v-for="(cell, cellIndex) in row"
                        :style="conditionalMerge(
                            !cssVars, {
                                height: cellHeight + 'px'
                            },
                            cell.style, cell.style
                        )">
                        {{ cell.value !== undefined ? cell.value : cell }}
                    </th>
                </tr>
            </thead>
        </table>
        <div class="scrollbar-spacing"/>
    </div>
</template>

<script>
export default {
    name: 'row-header',
    props: {
        data: {
            type: Array,
            default: []
        },
        offsetY: {
            type: Number,
            default: 0
        },
        spacingTop: {
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
        }
    },
    mounted () {
        this.$watch('offsetY', (offsetY) => {
            this.$el.scrollTop = offsetY
        })
    },
    methods: {
        getTotalWidth () {
            return this.$el.clientWidth
        },
        getMaxCellHeight () {
            return Math.max(...this.$refs.cell0.map(v => v.clientHeight))
        },
        conditionalMerge (cond1, object1, cond2, object2) {
            if (cond1 && cond2) {
                return Object.assign({}, object1, object2)
            } else if (cond1) {
                return object1
            } else if (cond2) {
                return object2
            }
            return null
        }
    }
}
</script>