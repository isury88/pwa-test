<template>
  <section class="container">
    <div>
      <logo/>
      <h1 class="title">
        NUXT
      </h1>
      <h2 class="subtitle">
        PWA Application
      </h2>
      <div :class="['network',online ? 'online' : 'offline']">
        <div class="circle"></div>
        {{ online ? 'online' : 'offline' }}
      </div>
      <br />

      <button class="btn btn-primary" @click="checkOs">Check OS</button>
    </div>
  </section>
</template>

<script>
  import Logo from '~/components/Logo.vue'

  export default {
    components: {Logo},
    data () {
      return {
        online: true
      }
    },
    mounted () {
      if (!window.navigator) {
        this.online = false
        return
      }
      this.online = Boolean(window.navigator.onLine)
      window.addEventListener('offline', this._toggleNetworkStatus)
      window.addEventListener('online', this._toggleNetworkStatus)
    },
    methods: {
      _toggleNetworkStatus ({ type }) {
        this.online = type === 'online'
      },
      checkOs() {
        let nav = window.navigator;
        let ua = nav.userAgent;

        function isiOsSafari (a) {
            return ("standalone" in nav) // There's a thing called standalone in nav
             && !nav.standalone // It is not running in standalone mode
             && ua.indexOf(a)!=-1 // iPhone is in the UA string (could be Opera)
             && ua.indexOf('Mac OS')!=-1 // There's Mac in the UA string (not Opera)
             && ua.indexOf('Safari')!=-1
             /* if all the above are true this probably means this is
             the Safari browser,
             not a webview in an app,
             not a page in standalone mode */
        }

        // Check if Mobile Safari on iPhone
        if(isiOsSafari('iPhone')){
            document.write('Probably Safari on an iPhone: ' + ua);
        }
        // Check if Mobile Safari on iPod
        else if(isiOsSafari('iPad')){
            document.write('Probably Safari on an iPad: ' + ua);
        }
        else{
            document.write('Probably something else: ' + ua)
        }
      }
    },
    destroyed () {
      window.removeEventListener('offline', this._toggleNetworkStatus)
      window.removeEventListener('online', this._toggleNetworkStatus)
    }
  }
</script>

<style>
  .container {
    min-height: 100vh;
    display: flex;
    justify-content: center;
    align-items: center;
    text-align: center;
  }

  .title {
    font-family: "Quicksand", "Source Sans Pro", -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, "Helvetica Neue", Arial, sans-serif; /* 1 */
    display: block;
    font-weight: 300;
    font-size: 100px;
    color: #35495e;
    letter-spacing: 1px;
  }

  .subtitle {
    font-weight: 300;
    font-size: 42px;
    color: #526488;
    word-spacing: 5px;
    padding-bottom: 15px;
  }

  .links {
    padding-top: 15px;
  }

  .network {
    font-weight: 400;
    font-size: 1rem;
  }

  .network .circle {
    display: inline-block;
    width: 1rem;
    height: 1rem;
    background: green;
    padding: .1rem .5rem;
    border-radius: 1rem;
  }

  .network.offline .circle {
    background: red;
  }
</style>
