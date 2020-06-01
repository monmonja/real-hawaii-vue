<template>
  <ul class="pagination">
    <li v-if="this.currentPage !== 1" class="with-hover" >
      <a :href="url + '?page=1'" @click.prevent="gotoPage(currentPage - 1)" class="active">PREV</a>
    </li>
    <li v-if="Math.max(this.currentPage - this.paddingCount, 1) === this.paddingCount"  :class="{active: currentPage === 1}" >
      <a :href="url + '?page=1'" @click.prevent="gotoPage(1)" class="active" data-page="1">1</a>
    </li>
    <li v-if="Math.max(this.currentPage - this.paddingCount, 1) > this.paddingCount"><a @click.prevent="">...</a></li>

    <li v-for="i in this.pagesInMiddle"  :class="{active: currentPage === i}" >
      <a :href="url + '?page=' + i" @click.prevent="gotoPage(i)">{{ i }}</a>
    </li>

    <li v-if="Math.min(this.currentPage + this.paddingCount, this.lastPage) < this.lastPage - 1"><a @click.prevent="">...</a></li>
    <li v-if="Math.min(this.currentPage + this.paddingCount, this.lastPage) < this.lastPage" :class="{active: currentPage === lastPage}">
      <a :href="url + '?page=' + this.lastPage" @click.prevent="gotoPage(lastPage)" >{{ this.lastPage }}</a>
    </li>

    <li v-if="this.currentPage !== this.lastPage" class="with-hover" >
      <a :href="url + '?page=' + (currentPage + 1)" @click.prevent="gotoPage(currentPage + 1)"  >NEXT</a>
    </li>
  </ul>
</template>

<script lang="ts">
  /**
   * <pagination :current-page="currentPage" :last-page="lastPage" url="/admin/users" :goto-page="getAllUsers"></pagination>
   */
  import {Component, Prop} from "vue-property-decorator";
  import Vue from "vue";

  @Component({
    name: 'Pagination'
  })
  export default class Pagination extends Vue {
    @Prop({ default: 2 }) readonly paddingCount:number
    @Prop({ default: 1, required: true }) readonly currentPage:number
    @Prop({ default: 1 }) readonly lastPage:number
    @Prop() readonly url:number
    @Prop() readonly gotoPage:Function

    get pagesInMiddle(): Array<number> {
      let start = Math.max(this.currentPage - this.paddingCount, 1);
      let end = Math.min(this.currentPage + this.paddingCount, this.lastPage);
      return  Array(end - start + 1).fill(1).map((_, idx) => start + idx);
    }
  }
  // export default {

  //   computed: {
  //     pagesInMiddle () {
  //       let start = Math.max(this.currentPage - this.paddingCount, 1);
  //       let end = Math.min(this.currentPage + this.paddingCount, this.lastPage);
  //       return  Array(end - start + 1).fill().map((_, idx) => start + idx);
  //     }
  //   }
  // }
</script>

