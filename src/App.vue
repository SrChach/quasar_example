<template>
  <div id="q-app">
    <div>
      <h2 v-if="!$q.platform.is.electron">Lo sentimos, esta aplicación está diseñada solo para Escritorio y Móvil</h2>
      <router-view v-else/>
    </div>
  </div>
</template>

<script>
export default {
  name: 'App',
  created () {
    this.activateSerial()
    this.validateSession()
    this.$router.push('/')
    this.$q.electron.ipcRenderer.on('close-window', this.logout)
  },
  beforeDestroy () {
    this.$q.electron.ipcRenderer.removeListener('close-window', this.logout)
  },
  methods: {
    validateSession () {
      const shaCode = localStorage.getItem('sha-code')
      const clientName = localStorage.getItem('client-name')
      this.$store.commit('security/setIsAuthenticated', shaCode)
      this.$store.commit('security/setClientName', clientName)
    },
    async activateSerial () {
      const serial = localStorage.getItem('serial-code')
      this.$store.commit('security/setIsActivated', serial)
    },
    logout () {
      this.$store.commit('security/logout')
    }
  }
}
</script>
