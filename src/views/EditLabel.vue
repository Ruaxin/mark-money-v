<template>
  <Layout>
    <div class="navBar">
      <Icon class="leftIcon" name="left" @click="goBack"/>
      <span class="title">编辑标签</span>
      <span class="rightIcon"/>
    </div>
    <div class="form-wrapper">
      <FormItem field-name="标签名" :value="currentTag.name" @update:value="update" placeholder="请输入标签名"/>
    </div>
    <div class="button-wrapper">
      <Button @click="remove">删除标签</Button>
    </div>
  </Layout>
</template>

<script lang="ts">
import Vue from 'vue';
import {Component} from 'vue-property-decorator';
import FormItem from '@/components/Money/FormItem.vue';
import Button from '@/components/Button.vue';

@Component({
  components: {Button, FormItem},
})
export default class EditLabel extends Vue {
  get currentTag() {
    return this.$store.state.currentTag;
  }

  get tagList() {
    return this.$store.state.tagList;
  }

  created() {
    const id = this.$route.params.id;
    this.$store.commit('fetchTags');
    this.$store.commit('setCurrentTag', id);
    if (!this.currentTag) {
      this.$router.replace('/404');
    }
  }

  update(name: string)
  {
    if (this.currentTag) {
      const idList = this.$store.state.tagList.map((item: Tag) => item.id);
      if (idList.indexOf(this.currentTag.id) >= 0) {
        this.$store.state.tagList.map((item: Tag) => item.name);
        const tag = this.$store.state.tagList.filter((item: Tag) => item.id === this.currentTag.id)[0];
        tag.name = name;
        this.$store.commit('saveTags');
      }
    }
  }


  remove() {
    if (this.currentTag) {
      this.$store.commit('removeTag', this.currentTag.id);
    }
  }

  goBack() {
    const names: string[] = (this.tagList.map((item: Tag) => item.name));
    let index = names.indexOf(this.currentTag.name);
    names.splice(index, 1);
    if(names.indexOf(this.currentTag.name) >= 0){
      window.alert('标签重复了');
    }else {
      this.$router.back();
    }
  }
}
</script>

<style lang="scss" scoped>
  .navBar {
    text-align: center;
    font-size: 16px;
    padding: 12px 16px;
    background: white;
    display: flex;
    align-items: center;
    justify-content: space-between;

    .leftIcon {
      width: 24px;
      height: 24px;
    }

    .rightIcon {
      width: 24px;
      height: 24px;
    }
  }

  .form-wrapper {
    background: white;
    margin-top: 8px;
  }

  .button-wrapper {
    text-align: center;
    padding: 16px;
    margin-top: 28px;
  }
</style>