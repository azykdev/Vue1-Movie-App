<template>
  <Box>
    <h3>Yangi kino qo'shish</h3>
    <form class="add-form d-flex justify-content-between" @submit.prevent>
      <Input
        type="text"
        class="new-movie-label"
        placeholder="Qanday kino?"
        v-model="name"
      />
      <Input
        type="number"
        class="new-movie-label"
        placeholder="Nechi marotaba ko'rilgan?"
        v-model="viewers"
      />
      <Button class="btn-outline-dark" type="submit" @click="addMovie">Qo'shish</Button>
    </form>
  </Box>
</template>

<script>
import Input from '../../ui-components/Input.vue';

export default {
    data() {
        return {
            name: "",
            viewers: "",
        };
    },
    methods: {
        addMovie() {
            if (!this.name || !this.viewers)
                return;
            const newMovie = {
                id: Date.now(),
                name: this.name,
                viewers: this.viewers,
                favourite: false,
                like: false,
            };
            this.$emit('createMovie', newMovie);
            this.name = '';
            this.viewers = '';
        }
    },
    components: { Input }
};
</script>

<style scoped>

input {
  width: 35%;
}
</style>
