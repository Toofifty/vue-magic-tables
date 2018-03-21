<template>
    <div
        class="magic-table__top-left__container"
        v-once>
        <table
            class="magic-table__top-left__table">
            <thead
                class="magic-table__top-left__head">
                <tr
                    class="magic-table__top-left__row"
                    :key="rowIndex"
                    v-for="(row, rowIndex) in data">
                    <th
                        class="magic-table__top-left__cell"
                        :key="cellIndex"
                        v-for="(cell, cellIndex) in row"
                        :style="!cssVars ? {
                                height: headerHeight + 'px'
                            } : null">
                        {{ cell }}
                    </th>
                </tr>
            </thead>
        </table>
    </div>
</template>

<script>
export default {
    name: 'top-left',
    props: {
        data: {
            type: Array,
            default: () => []
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
    methods: {
        getTotalHeight () {
            return this.$el.clientHeight
        }
    }
}
</script>