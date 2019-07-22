<template>
  <el-container>
    <el-header>
      <el-menu
        :default-active="'2'"
        class="el-menu-demo"
        mode="horizontal"
        background-color="#545c64"
        text-color="#fff"
        active-text-color="#ffd04b"
        @select="handleSelect"
        :router="true"
      >
        <el-menu-item index="1" route="/">
          epic7-tierlist
        </el-menu-item>
        <el-menu-item index="2" route="/">
          PvE
        </el-menu-item>
        <el-menu-item index="3" route="pvp">
          PvP
        </el-menu-item>
        <el-menu-item index="4" route="credits">
          Credits
        </el-menu-item>
      </el-menu>
    </el-header>
    <el-main>
      <nuxt />
    </el-main>
  </el-container>
</template>

<style>
.el-header {
  padding: 0;
}

.el-table .cell {
  white-space: inherit;
  word-break: break-word;
}

@media ( max-width: 767px ) {
  .el-main {
    padding: 0;
  }
  .el-card__body {
    padding: 0;
  }
}

html {
  font-family: 'Source Sans Pro', -apple-system, BlinkMacSystemFont, 'Segoe UI',
    Roboto, 'Helvetica Neue', Arial, sans-serif;
  font-size: 16px;
  word-spacing: 1px;
  -ms-text-size-adjust: 100%;
  -webkit-text-size-adjust: 100%;
  -moz-osx-font-smoothing: grayscale;
  -webkit-font-smoothing: antialiased;
  box-sizing: border-box;
}

*,
*:before,
*:after {
  box-sizing: border-box;
  margin: 0;
}
</style>

<script>
export default {
  methods: {
    handleSelect(key, keyPath) {
      // console.log(key, keyPath);
    }
  }
}
</script>
