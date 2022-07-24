<template>
  <div class="container-fluid">
    <header class="header">
        <div class="container">
          <div class="header__text">Сравнение исходного кода</div>
        </div>
    </header>
     <MonacoEditor/>
    
  </div>
</template>

<script>
import MonacoEditor from './components/MonacoEditor.vue'

export default {
  name: 'App',
  components: {
    MonacoEditor
}
}
</script>

<style lang="scss">


html {
  body {
  background-color: #f5f5f5;
}
}

.header {
  height: 70px;
  border-bottom: 1px solid #DEDEDE;
  align-items: center;
  display: flex;

  &__text {
    color: #666666;
  }
}
</style>