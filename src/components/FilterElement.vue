<template>
    <div>
        <h2>{{filterName}}</h2>
        <select :name="filterName"  @change="onChange($event)">
            <option v-for="(option,key) in options" :value="key">
                {{option.label}}
            </option>
        </select>
    </div>
</template>

<script>
export default{
    props: {
        filterName: String,
        options: Array,
    },
    methods: {
        onChange(event) {
            this.$emit('newFilter',event.target.value);
        }
    }
}
</script>