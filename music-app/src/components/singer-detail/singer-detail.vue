<template>
	<transition name='slide'>
		<div class="singer-detail"></div>
	</transition>
  
</template>

<script type="text/ecmascript-6">
  import {mapGetters} from 'vuex'
  import {getSingerDetail} from '@/api/singer'
  import {ERR_OK} from '@/api/config'
  import {createSong} from '@/common/js/song'
  // import {mapGetters} from 'vuex'

  export default {
  	data() {
  		return {
  			songs: []
  		}
  	},
    computed: {
    	...mapGetters([
    		'singer'
    	])
    },
    created() {
    	this._getDetail()
    },
    methods: {
    	_getDetail() {
    		if(!this.singer.id){
    			this.$router.push('/singer')
    			return
    		}
    		getSingerDetail(this.singer.id).then((res) => {
    			if(res.code = ERR_OK){
    				this.songs = this._normalizeSongs(res.data.list)
    				console.log(this.songs)	
    			}
    		})
    	},
    	_normalizeSongs(list) {
    		let ret = []
    		list.forEach((item) =>{
    			let {musicData} = item
    			if(musicData.songid && musicData.ablummid) {
    				ret.push(createSong(musicData))
    			}
    		})
    		return ret
    	}
    }
  }
</script>

<style scoped lang="stylus" rel="stylesheet/stylus">	
	@import "../../common/stylus/variable.styl"
  	.singer-detail
		position: fixed
		top: 0
		bottom: 0
		right: 0
		left: 0
		background: red
		
	.slide-enter-active, .slide-leave-active
		transition: all 0.3s
	
	.slide-enter, .slide-leave-to
		transform: translate3d(100%, 0, 0)
</style>