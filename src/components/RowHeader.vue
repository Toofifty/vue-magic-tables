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
                        :ref="'cell' + rowIndex"
                        :key="cellIndex"
                        v-for="(cell, cellIndex) in row"
                        :style="!cssVars ? {
                                height: cellHeight + 'px'
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
        }
    }
}
</script>