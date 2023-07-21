<template>
  <div v-if="infoItem" class="comments__info">
    <h1>Full message details</h1>
    <p><strong>Id</strong>: {{ infoItem.id }}</p>
    <p><strong>Name</strong>: {{ infoItem.name }}</p>
    <p><strong>Email</strong>: {{ infoItem.email }}</p>
    <p><strong>Comments</strong>: {{ infoItem.body }}</p>

    <NuxtLink to="/">
      <button class="btn__back">Back</button>
    </NuxtLink>
  </div>

  <p class="loading" v-else>Loading...</p>
</template>

<script>
export default {
  name: "TheComments",

  data: () => {
    return {
      infoItem: null,
    };
  },

  created() {
    fetch(
      `https://jsonplaceholder.typicode.com/comments/${this.$route.query.id}`
    )
      .then((response) => response.json())
      .then((res) => (this.infoItem = res));
  },
};
</script>

<style scoped>
.comments__info {
  padding: 50px;
  box-shadow: 0px 0px 2px 2px var(--greyLight);
  margin: 50px;
  border-radius: 5px;
  position: relative;
}
.comments__info h1 {
  color: var(--greyDark);
}
.loading {
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  font-size: 2rem;
  color: var(--primary);
}

.btn__back {
  background-color: var(--primary);
  padding: 5px 15px;
  color: var(--greyLight-2);
  border-radius: 4px;
  font-size: 1.5rem;
  border: none;
  cursor: pointer;
  position: absolute;
  bottom: 20px;
  right: 50px;
}
.btn__back:hover {
  background-color: var(--greyLight);
  color: var(--greyDark);
}
</style>