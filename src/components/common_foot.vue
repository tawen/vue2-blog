<template>
<div class='footer'>
  <router-link to="/about"><span>关于我</span></router-link>|<span><router-link to="/diary/d_edit">发表动态</router-link></span>|<span><router-link to="/message_board/m_edit">给我留言</router-link></span>
</div>
</template>
<script>

</script>
<style scoped>
.footer {
  width: 100%;
  position: fixed;
  bottom: 0;
  color: #000000;
  height: 2rem;
  z-index: 555;
  text-align: center;
  font-size: .7rem;
  letter-spacing: 0.3rem;
  line-height: 2rem;
}

.footer a {
  text-decoration: none;
}
</style>
