<template>
    <a-menu
      mode="inline"
      theme="dark"
      :inlineCollapsed="collapsed"
      @click="handleClick"
    >
      <template v-for="item in routes">
        <a-menu-item v-if="!item.children" :key="parentPath ? parentPath + '/' + item.path : item.path">
          <a-icon :type="item.meta.icon"  v-if="item.meta.icon!=''||item.meta.icon!=undefined"/>
          <span>
          {{ item.meta?item.meta.title:'未设定meta属性title' }}
        </span>
        </a-menu-item>
        <a-menu-item v-else-if="item.children.length==1&&!item.children[0].children" :key="parentPath ? parentPath + '/' + item.children[0].path : ((item.path=='/'?'':item.path) +'/'+ item.children[0].path)">
          <a-icon :type="item.children[0].meta.icon"  v-if="item.meta.icon!=''||item.meta.icon!=undefined"/>
          <span>
          {{item.children[0].meta.title}}
        </span>
        </a-menu-item>
        <sub-menu v-else :menu-info="item"
          :key="parentPath ? parentPath + '/' + item.path : item.path"
          :parentPath="parentPath ? parentPath + '/' + item.path : item.path"
          />
      </template>
    </a-menu>
</template>

<script>
import SubMenu from './SubMenu'
export default {
  components: {
    'sub-menu': SubMenu,
  },
  props:["parentPath"],
  data () {
    return {
      routes: global.antRouter,
      collapsed: true
    }
  },
  methods: {
    toggleCollapsed () {
      this.collapsed = !this.collapsed
    },
    handleClick(e){
      console.log(e);
      var path = e.key;
      this.$router.push(path)
    }
  },
}
</script>
<style lang="scss" scoped>
.ant-menu-item{
  text-align: left
}
</style>