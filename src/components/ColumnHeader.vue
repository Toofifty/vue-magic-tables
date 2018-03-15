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
                        :ref="'cell' + rowIndex"
                        :key="cellIndex"
                        v-for="(cell, cellIndex) in row"
                        :style="!varStyles ? {
                                minWidth: cellWidth + 'px'
                            } : null">
                        {{ cell }}
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
        varStyles: {
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
        }
    }
}
</script>