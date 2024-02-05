<script setup lang="ts"></script>

<template>
  <div>
    <nav>
      <div class="nav-bar">
        <router-link to="/">Home</router-link>
        <router-link to="/contact">Contact</router-link>
        <router-link to="/about">About</router-link>
      </div>
    </nav>
    <section class="public-layout">
      <router-view />
    </section>
  </div>
</template>

<style>
.nav-bar {
  display: flex;
  justify-content: right;
  align-items: center;
  gap: 24px;
}
.public-layout {
  width: 100%;
  flex: 1;
}
body {
  display: flex;
}
</style>
