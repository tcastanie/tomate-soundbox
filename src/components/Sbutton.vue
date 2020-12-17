<template>
    <v-btn :id="name" @click="play" block class="justify-start">
        <v-icon v-if="!btnPause" class="mr-2">mdi-play</v-icon>
        <v-icon v-else class="mr-2">mdi-pause</v-icon>
        <span>{{desc}}</span>
    </v-btn>
</template>

<script>
export default {
    name: "Sbutton",
    props: ["name", "desc", "playing"],
    data: () => ({
        btnPause: false,
    }),
    methods: {
        play() {
            if (!this.btnPause) {
                this.audio.play();
                this.btnPause = true;
                this.$emit("playing", this.name);
                var soundPlaying = setInterval(() => {
                    let progression = Math.round(
                        (this.audio.currentTime / this.audio.duration) * 100
                    );
                    this.$emit("time", progression);
                    if (this.audio.ended || !this.playing) {
                        this.btnPause = false;
                        this.$emit("stopped", this.name);
                        clearInterval(soundPlaying);
                    }
                }, 250);
            } else {
                this.audio.pause();
                this.btnPause = false;
            }
        },
        stop() {
            this.audio.pause();
            this.audio.currentTime = 0;
        },
    },
    computed: {
        audio() {
            return new Audio(require(`@/assets/${this.name}.mp3`));
        },
    },
    watch: {
        playing() {
            if (this.playing === false) {
                this.stop();
            }
        },
    },
};
</script>

<style>
</style>