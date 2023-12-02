<template>
  <Input
    type="text"
    class="search-input p-3 mb-3"
    placeholder="Kinolarni qidirish"
    v-model="term"
    @input="searchFunc"
  />
</template>

<script>
import Input from '../../ui-components/Input.vue';

export default {
    props: {
        updateTermHandler: {
            type: Function,
            required: true,
        },
    },
    data() {
        return {
            term: ''
        };
    },
    methods: {
        searchFunc(e) {
            this.term = e.target.value;
            this.updateTermHandler(this.term);
        }
    },
    components: { Input }
};
</script>

<style scoped></style>
