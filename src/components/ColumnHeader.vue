<template>
    <div
        class="magic-table__header-column__container"
        v-once>
        <table
            class="magic-table__header-column__table">
            <thead
                class="magic-table__header-column__head">
                <tr
                    class="magic-table__header-column__row"
                    :key="rowIndex"
                    v-for="(row, rowIndex) in data">
                    <th
                        class="magic-table__header-column__cell"
                        :class="cell.class ? cell.class : null"
                        :ref="'cell' + rowIndex"
                        :key="cellIndex"
                        v-for="(cell, cellIndex) in row"
                        :style="conditionalMerge(
                            !cssVars, {
                                minWidth: cellWidth + 'px',
                                height: headerHeight + 'px'
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
    name: 'column-header',
    props: {
        data: {
            type: Array,
            default: []
        },
        offsetX: {
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
        headerHeight: {
            type: Number,
            default: 0
        },
        cssVars: {
            type: Boolean,
            default: false
        }
    },
    mounted () {
        this.$watch('offsetX', (offsetX) => {
            this.$el.scrollLeft = offsetX
        })
    },
    methods: {
        getTotalHeight () {
            return this.$el.clientHeight
        },
        getMaxCellWidth () {
            return Math.max(...this.$refs.cell0.map(v => v.clientWidth))
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