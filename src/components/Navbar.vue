<template>
  <nav class="navbar navbar-dark bg-dark justify-content-between">
    <router-link to="/" class="navbar-brand">Student App</router-link>
    <div class="navbar-nav">
      <li class="nav-item mr-auto">
        <router-link to="/new" class="nav-link">Add Student</router-link>
      </li>
    </div>
  </nav>
</template>
<script>
export default {
  name: "navbar"
};
</script>