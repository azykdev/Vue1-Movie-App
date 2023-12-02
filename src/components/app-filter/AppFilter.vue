<template>
  <div class="btn-group">
    <Button
      type="button"
      v-for="btn in filterButtons"
      :key="btn.name"
      :class="[filter == btn.name ? 'btn-dark' : 'btn-outline-dark']"
      @click="filterHandler(btn.name)"
    >
      {{ btn.title }}
  </Button>
    
  </div>
</template>

<script>
export default {
    data() {
        return {
            filterButtons: [
                {
                    title: "Barcha kinolar",
                    name: "all",
                },
                {
                    title: "Mashhur kinolar",
                    name: "popular",
                },
                {
                    title: "Eng ko'p ko'rilgan kinolar",
                    name: "mostViewers",
                },
            ],
            filter: "all",
        };
    },
    methods: {
        filterHandler(filter) {
            this.filter = filter;
            this.$emit("updateFilter", this.filter);
        },
    },
};
</script>

<style scoped></style>
