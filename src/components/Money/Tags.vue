<template>
  <div class="tags">
    <div class="new">
      <button @click="createTag">新增标签</button>
    </div>
    <ul class="current">
      <li v-for="tag in tagList" :key="tag.id"
          :class="{selected:selected(tag)}"
          @click="toggle(tag)">{{tag.name}}
      </li>
    </ul>
  </div>

</template>


<script lang="ts">
  import {Component} from 'vue-property-decorator';
  import {mixins} from 'vue-class-component';
  import {TagHelper} from '@/mixins/TagHelper';

  @Component
  export default class Tags extends mixins(TagHelper) {

    selectedTags: Tag[] = [];
    get tagList(){
      return  this.$store.state.tagList
    }

    selected(tag: Tag){
      const selectedTagsName = this.selectedTags.map(items => items.name)
      return selectedTagsName.indexOf(tag.name)>=0
    }
    toggle(tag: Tag) {
      const selectedTagsName = this.selectedTags.map(items => items.name)
      const index = selectedTagsName.indexOf(tag.name);
      if (index >= 0) {
        this.selectedTags.splice(index, 1);
      } else {
        this.selectedTags.push(tag);
      }
      this.$emit('update:value', this.selectedTags);
    }


  }
</script>

<style lang="scss" scoped>
  @import "src/assets/style/helper";
  .tags {
    font-size: 14px;
    padding: 16px;
    flex-grow: 1;
    display: flex;
    flex-direction: column-reverse;
    background-color: white;

    > .current {
      display: flex;
      flex-wrap: wrap;

      > li {
        $beSelected-color-grey:#d3d3d3;
        background:$beSelected-color-grey;
      ;
        $h: 24px;
        height: $h;
        line-height: $h;
        border-radius: $h/2;
        padding: 0 16px;
        margin-right: 12px;
        margin-top: 4px;
        transition: all 0.3s ease;


        &.selected {
          background-color:darken($beSelected-color,45%);
          color: white;
          box-shadow: 3px 6px 5px 0 rgba(0,0,0,.2);
          transform: scale(1.1) translateX(-1px) translateY(-1px);
        }
      }
    }

    > .new {
      padding-top: 16px;

      button {
        background: transparent;
        border: none;
        color: #999;
        border-bottom: 1px solid;
        padding: 0 4px;
      }
    }
  }
</style>