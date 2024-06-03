<template>
    <div class="error-page">
        <h2>Oops!</h2>
        <h3>404</h3>
        <p>Page Not Found</p>
        <p>Go to <router-link :to="`/`">Home page</router-link></p>
    </div>
    </template>

<style>
.error-page {
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    min-height: 50vh;
    gap: 1rem;
}

.error-page h2 {
    font-size: 4rem;
    color: #132f42;
}

.error-page h3 {
    font-size: 2rem;
    font-style: italic;
}

</style>