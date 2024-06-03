<template>
    <div>
    <div class="repo-card">
      <h2 class="skeleton hdn"></h2>
      <div class="skeleton pgph"></div>
      <div class="skeleton pgph"></div>
      <div class="skeleton pgph"></div>
    </div>
  </div>
</template>

<style>
.skeleton {
  width: 100%;
  height: 1rem;
  border-radius: 0.2rem;
  animation: pulse-bg 1.8s infinite;
  margin: 1rem 0;
}

.hdn {
  width: 100%;
  height: 1.5rem;
}

.pgph {
  width: 40%;
  height: 1rem;
}

@keyframes pulse-bg {
  0% {
    background-color: #80808024;
  }

  50% {
    background-color: #80808038;
  }

  100% {
    background-color: #80808024;
  }
}
</style>
