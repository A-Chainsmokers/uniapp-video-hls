<template>
	<view class="content"><video ref="video" :src="src" enable-play-gesture	 class="video-class"></video></view>
</template>

<script>
const Hls = require('hls.js');
export default {
	data: () => ({
		url: 'https://new.iskcd.com/20220129/vFXylskt/1100kb/hls/index.m3u8',
		hls: null,
		src: undefined
	}),
	mounted() {
		this.init();
	},
	methods: {
		init() {
			if (Hls.isSupported()) {
				this.hls = new Hls();
				this.hls.loadSource(this.url);
				try{
					this.hls.attachMedia(this.$refs.video);
				}catch(e){
					console.log("uniapp video组件src不能为空")
				}
				

				this.hls.on(Hls.Events.MANIFEST_PARSED, () => {
					console.log('加载成功');
				});
				this.hls.on(Hls.Events.ERROR, (event, data) => {
					console.log('加载失败');
				});
			}
		}
	},
	destroyed() {
		if (this.hls) {
			this.hls.destroy();
		}
	}
};
</script>

<style>
.video-class {
	margin: 0 auto;
}
</style>
