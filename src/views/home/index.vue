<template>
 <el-container>
     <el-aside>
         <layoutAside></layoutAside>
     </el-aside>
     <el-container>
         <el-header></el-header>
         <el-main></el-main>
     </el-container>
 </el-container>
</template>

<script>
import layoutAside from '../../components/home/layout-aside'
export default {
  components: {
    'layoutAside': layoutAside
  }
}
</script>

<style>
</style>
