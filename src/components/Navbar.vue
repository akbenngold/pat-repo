<script setup>
   import gitHubIcon from './icons/gitHubIcon.vue';
</script>

<template>
    <nav>
        <div class="navbar">
            <gitHubIcon class="github-icon" />
            <h1>Patie</h1>
        </div>
    </nav>
</template>

<style scoped>

.navbar {
  display: flex;
  align-items: center;
  gap: 10px;
  color: #fff;
  justify-content: center;
  background-color: #232c53;
  text-align: center;
  padding: 20px;
  margin-bottom: 20px;
}

.github-icon {
  font-size: 3rem;
}

</style>