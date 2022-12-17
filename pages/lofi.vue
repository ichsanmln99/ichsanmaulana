<template>
    <div class="lofi">
        <div class="w-full h-full fixed flex flex-col justify-between p-10 text-2xl z-40 opacity-90">
            <div>
                <NuxtLink class="text-shadow-green bojji" to="/"
                >Bojjidev</NuxtLink>
            </div>
            <div class="flex justify-center">
                <div ref="playbutton" class="w-96 h-96 flex items-center justify-center group cursor-pointer" @click="setAction()">
                    <img :src="isPaused ? 'play.svg' : 'pause.svg'" class="h-[24px] w-auto shadow-green hidden group-hover:block" alt="">
                </div>
            </div>
            <div class="flex">
                <a class="text-shadow-green cursor-pointer" @click="pickRandomVideo()">
                    Now playing <span class="blink">:</span> {{currentVideo.video_name}}
                </a>
            </div>
        </div>

        <div class="lofi-screen fixed w-screen h-screen">
            <div class="lines z-10"></div>
            <div class="vignette z-20"></div>
            <div class="noise z-30"></div>
            <img :src="bgImage" class="lofi-image object-cover select-none ">
            <client-only>
                <div class="hidden">
                    <LazyYoutube v-if="currentVideo.video_url" ref="videoPlayer" :src="currentVideo.video_url" autoplay></LazyYoutube>
                </div>
            </client-only>
        </div>
    </div>
</template>

<script>

export default {
    name: "Lofi",
    async asyncData({ $content }) {
		const lofis = await $content('lofis').fetch();

		return {
			lofis,
		}
	},
    data(){
        return {
            currentVideo:{},
            isPaused:false
        }
    },
    fetch(){
        this.pickRandomVideo();
    },
    computed:{
        bgImage(){
            const randomNumber = Math.floor(Math.random() * 8) + 1;

            return `lofi-${randomNumber}.gif` 
        }
    },
    destroyed(){
        window.removeEventListener('keypress', this.setActionOnSpaceKey);
    },
    mounted(){
        window.addEventListener('keypress', this.setActionOnSpaceKey);
    },
    methods: {
        pickRandomVideo(){
            const videos = this.lofis.data;
            const randomVideo = videos[Math.floor(Math.random() * videos.length)];

            this.currentVideo = randomVideo

        },
        setActionOnSpaceKey(e){
            if(e.code === 'Space' || e.key === " " || e.keyCode === 32){
                this.setAction()
            }
        },
        setAction(){
            this.isPaused ? this.playVideo() : this.pauseVideo() 
        },
        pauseVideo(){
            this.isPaused = true
            this.$refs.videoPlayer.pauseVideo()
            this.hideUnHidePlaybutton();
        },
        playVideo(){
            this.isPaused = false
            this.$refs.videoPlayer.playVideo()
            this.hideUnHidePlaybutton();
        },
        hideUnHidePlaybutton(){
            this.$refs.playbutton.children[0].classList.remove('hidden');
            setTimeout(() => {
                this.$refs.playbutton.children[0].classList.add('hidden');
            }, 500);
        }

    }
}

</script>


<style scoped>
    .bojji::after {
        content: ".";
        @apply text-4xl text-shadow-green blink
    }

    .blink {
        animation: blink 1s step-start 0s infinite;
      }

    .lofi {
        font-family: 'vhs';
    }

    .vignette {
        background: rgb(0,0,0);
        background: radial-gradient(circle, rgba(0,0,0,0) 0%, rgba(0,0,0,0.7) 100%);
        position: absolute;
        height: 100%;
        width: 100%;
    }

    .text-shadow-green {
        text-shadow: 0px 0px 10px rgb(38, 255, 31);
    }

    .shadow-green {
        -webkit-filter: drop-shadow( 0px 0px 10px rgb(38, 255, 31));
        filter: drop-shadow( 0px 0px 10px rgb(38, 255, 31));
      
    }


    .lines {
        position: absolute;
        height: 100%;
        width: 100%;
        pointer-events: none;
        opacity: 0.6;
        will-change: opacity;
        animation: opacity 3s linear infinite;
       }
       
       .lines:before {
        content: "";
        position: absolute;
        left: 0;
        top: 0;
        right: 0;
        bottom: 0;
        pointer-events: none;
        background: linear-gradient(
         to bottom,
         transparent 50%,
         rgba(0, 0, 0, 0.5) 51%
        );
        background-size: 100% 4px;
        will-change: background, background-size;
        animation: scanlines 0.2s linear infinite;
       }

      .noise {
        position: absolute;
        height: 100%;
        width: 100%;
        background: transparent url('http://assets.iceable.com/img/noise-transparent.png') repeat 0 0;
        background-repeat: repeat;
        animation: noise .2s infinite;
        opacity: .9;
        visibility: visible;
      }

      .lofi-screen {
        left: 50%;
        top: 50%;
        transform: translate(-50%, -50%);
        animation: turnon 0.8s normal;
    }

      .lofi-image{
        position: absolute;
        z-index: -1;
        height: 100%;
        width: 100%;
        z-index: 0;
        background-color: white;
      }
       

/*Keyframes*/


@keyframes scanlines {
    from {
     background: linear-gradient(
      to bottom,
      transparent 50%,
      rgba(0, 0, 0, 0.5) 51%
     );
     background-size: 100% 6px;
    }
   
    to {
     background: linear-gradient(
      to bottom,
      rgba(0, 0, 0, 0.5) 50%,
      transparent 51%
     );
     background-size: 100% 6px;
    }
   }


   @keyframes opacity {
    0% {
     opacity: 0.6;
    }
   
    20% {
     opacity: 0.7;
    }
   
    35% {
     opacity: 0.6;
    }
   
    50% {
     opacity: 0.9;
    }
   
    60% {
     opacity: 0.5;
    }
   
    80% {
     opacity: 0.8;
    }
   
    100% {
     opacity: 0.5;
    }
   }

   @keyframes noise {
    0% { transform: translate(0,0) }
    10% { transform: translate(-5%,-5%) }
    20% { transform: translate(-10%,5%) }
    30% { transform: translate(5%,-10%) }
    40% { transform: translate(-5%,15%) }
    50% { transform: translate(-10%,5%) }
    60% { transform: translate(15%,0) }
    70% { transform: translate(0,10%) }
    80% { transform: translate(-15%,0) }
    90% { transform: translate(10%,5%) }
    100% { transform: translate(5%,0) }
}
    
    @keyframes turnon {
        0% {
            height: 1%;
            width: 1%;
            filter: brightness(300%);
        }

        40% {
            height: 1%;
            width: 100%;
            filter: brightness(300%);
        }



        100% {
            height: 100%;
            width: 100%;
            filter: brightness(100%);
        }

    }  
    
    @keyframes blink {
        50% {
          opacity: 0.0;
        }
      }
</style>