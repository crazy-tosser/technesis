<template>  

<div class="tagbar" 
  :class="{'center': lastVisibleId <=2 ? false: center, left}"
  ref="tagbarRef"

>
  <div
        class="tag"
        :class="{'separator': tag.separator, 'lastId': i == lastVisibleId }"
        ref="tagRef"
        v-for="(tag, i) in tagsWithDevaider"
        :key="i"        
      >
       
        <div class="tag-icon" v-if="tag.icon"><v-icon small>{{tag.icon}}</v-icon></div>
        <div class="tag-text" v-if="tag.text">{{tag.text}}</div>
        <div class="tag-separator" v-if="tag.separator"> <v-icon>mdi-circle-small</v-icon> </div>
     </div>

</div>

</template>

<script>


export default {
  name: 'TagBar',
  props: {
    tags: {
        type: Array,
        default: () => []
    },
    center:{
      type: Boolean,
      default: true
    },
    left:{
      type: Boolean,
      default: false
    }
  },
  data: () => ({

    event: null,
    lastVisibleId: -1,
  }),
  computed:{
    tagsWithDevaider() { 
      let newArr = []
      this.tags.forEach((el, i) => { 
        newArr.push(el)
        if (i != this.tags.length-1) newArr.push({separator: true})
      });
      return newArr
    },

  },
  mounted(){
    this.event = new ResizeObserver(()=>{this.getWidth()})
    this.event.observe(this.$refs.tagbarRef)
  },
  beforeDestroy () {
  this.event.unobserve(this.$refs.tagbarRef)
  },
  watch: {
    tags: function() {
      this.getWidth()
    }
    
  },
  methods:{
    getWidth(){
      this.$nextTick(()=>{
        const widthTagBar = this.$refs.tagbarRef.clientWidth
        let sumWidth = 0
        this.lastVisibleId=-1
        if (this.$refs.tagRef && this.tags.length > 1 )
          this.$refs.tagRef.some ((el, i) => {
            sumWidth = sumWidth+el.clientWidth + 5 // +gap
          
            if (sumWidth>widthTagBar){
              //console.log(el.classList.contains('separator'))
              //el.classList.contains('separator') ? this.lastVisibleId = i-1 : this.lastVisibleId = i
              //(i-1)%2 == 0 ? this.lastVisibleId = i-1 : this.lastVisibleId = i-2
              this.lastVisibleId = i-1
              return true
            }
          })
        //console.log( widthTagBar, sumWidth, this.lastVisibleId) 
      })
    }
  }
}
</script>


<style scoped lang="sass">
.tagbar
  display: flex
  flex-wrap: wrap
  align-items: center
  gap: 5px
  white-space: nowrap
  overflow: hidden
  max-height: 44px
  justify-content: space-around
  &.center
    justify-content: space-between
  &.left
    justify-content: flex-start

.tag
  display: flex
  padding: 10px   
  &.separator.lastId
    margin-left: 500px
    
  &-icon
    margin: 
      right: 5px

  &-separator
    margin: auto

  
</style>
