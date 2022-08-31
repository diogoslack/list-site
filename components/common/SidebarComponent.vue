<template>
  <nav id="sidebarMenu" class="col-md-3 col-lg-2 d-md-block align-items-left bg-white sidebar collapse">
    <div class="position-sticky pt-3">
      <ul class="nav flex-column">
        <li class="nav-item">
          <a class="nav-link" href="./">
            <list-icon size="1.5x" class="icon-class"></list-icon>
            List
          </a>
        </li>           
      </ul>
    </div>
  </nav>  
</template>

<script>
import { ListIcon } from 'vue-feather-icons'

export default {
  name: 'SidebarComponent',
  components: {
    ListIcon
  },
  middleware: 'auth',
  methods: {
    async logout (event) {
      await this.$auth.logout();
      window.location.href = `./`;
    }
  }
}
</script>

<style scoped>
.hide-menu {
  display: none;
}
</style>