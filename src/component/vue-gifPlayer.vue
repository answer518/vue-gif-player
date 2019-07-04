<template>
    <div ref="gifplayer" class="gifplayer-wrapper" :style="gifStyle">
        <div v-if="!isPlaying" class="gifplayer-preview">
            <img class="gifplayer" :src="src" @load="load">
            <div v-if="pending" class="spinner"></div>
            <ins class="play-gif" @click="playGif">GIF</ins>
        </div>
        <div v-else class="gifplayer-element">
            <img class="gif-element" :src="gifSrc" @click="stopGif">
        </div>
    </div>
</template>

<script>
import { supportFormats } from './utils'

export default {
    name: 'gif-Player',
    props: {
        src: {
            type: String,
            required: true
        },
        label: {
            type: String,
            required: false,
            default: 'GIF'
        },
        playOn: {
            type: String,
            required: false,
            default: 'click'
        },
        onPlay: {
            type: Function,
            default: () => {}
        },
        onStop: {
            type: Function,
            default: () => {}
        },
        onClick: {
            type: Function,
            default: () => {}
        },
        onLoad: {
            type: Function,
            default: () => {}
        },
        onLoadComplete: {
            type: Function,
            default: () => {}
        }
    },
    data() {
        return {
            isPlaying: false,
            gifStat: 'init',
            gifSrc: this.src,
            gifStyle: {}
        }
    },
    computed: {
        pending() {
            return this.gifStat === 'pending'
        },
        loaded() {
            return this.gifStat === 'loaded'
        }
    },
    mounted() {},
    methods: {
        playGif() {
            this.onClick()
            this.loadGif()
        },
        loadGif() {
            if (this.gifStat === 'loaded') {
                this.isPlaying = true
            } else {
                this.gifStat = 'pending'
                this.preLoadGif(this.getFile('gif'))
            }
        },
        preLoadGif(src) {
            const gifImg = new Image()
            gifImg.src = src
            gifImg.onload = () => {
                this.gifStat = 'loaded'
                this.gifSrc = src
                this.isPlaying = true
            }
            gifImg.onerror = () => {
                console.log('error')
            }
        },
        stopGif() {
            this.isPlaying = false
            this.onStop()
        },
        getFile(ext) {
            let gifSrc = this.gifSrc
            for (let i=0; i< supportFormats.length ;i++) {
                const pattrn = new RegExp( supportFormats[i] + '$', 'i')
                gifSrc = gifSrc.replace(pattrn, ext)
            }
            return gifSrc
        },
        load() {
            const gifplayerWrapper = this.$refs.gifplayer
            const gifplayer = gifplayerWrapper.getElementsByClassName('gifplayer')[0]
            this.gifStyle = {
                width: gifplayer.width + 'px',
                height: gifplayer.height + 'px'
            }
        }
    },
    destroyed() {}
}
</script>

<style>

.gifplayer-wrapper {
    position: relative;
}

.play-gif{
    position: absolute;
    left: 50%;
    top: 50%;
    margin-left: -25px;
    margin-top: -25px;
	font-family: Arial, sans serif;
	width: 50px;
	height: 50px;
	line-height: 52px;
	text-align: center;
	background: #222;
	font-size: 18px;
	color: #fff;
	border-radius: 50%;
	opacity: .9;
	border: 4px solid #fff;
	cursor:pointer;
	text-decoration: none;
}

.play-gif:hover{
	opacity:.5;
}

.spinner {
	height:50px;
	width:50px;
	margin:0px auto;
	position:absolute;
	top:50%;
	left:50%;
	margin-top:-25px;
	margin-left:-25px;
	animation: rotation .6s infinite linear;
	border-left:6px solid rgba(256,256,256,.15);
	border-right:6px solid rgba(256,256,256,.15);
	border-bottom:6px solid rgba(256,256,256,.15);
	border-top:6px solid rgba(256,256,256,.8);
	border-radius:100%;
}

@keyframes rotation {
	from {transform: rotate(0deg);}
	to {transform: rotate(359deg);}
}

</style>
