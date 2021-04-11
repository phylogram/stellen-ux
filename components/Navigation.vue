<template>
  <b-navbar toggleable="lg" type="light">
    <b-container>
      <b-navbar-brand href="/">
        <img
          src="@/assets/images/fundament_logo.svg"
          class="mr-2"
          alt="MMP Stellen-App"
          itemprop="logo"
        />
        MMP Stellen-App
      </b-navbar-brand>

      <b-navbar-toggle target="nav-collapse"></b-navbar-toggle>

      <b-collapse id="nav-collapse" is-nav>
        <b-navbar-nav>
          <b-link to="/app/search" class="nav-link">App</b-link>
          <b-link to="/about" class="nav-link">About</b-link>
          <b-link to="/feedback" class="nav-link">Feedback</b-link>
        </b-navbar-nav>

        <!-- Right aligned nav items -->
        <b-navbar-nav class="ml-auto">
          <Search></Search>
        </b-navbar-nav>
      </b-collapse>
    </b-container>
  </b-navbar>
</template>
<script>
export default {
  name: 'Navigation',
}
</script>
