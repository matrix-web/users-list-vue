<template>
  <div class="not-found">
    <h1>Page Not Found 404</h1>
    <div>
      <router-link to="/">Перейти на главную</router-link>
    </div>
  </div>
</template>

<style>
.container--fluid {
  height: 100%;
}
.not-found {
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  height: 100%;
}
</style>
