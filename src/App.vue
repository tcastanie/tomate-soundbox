<template>
    <v-app>
        <v-app-bar app color="#E53935" dark>
            <v-app-bar-nav-icon class="text-h5">🍅</v-app-bar-nav-icon>
            <v-toolbar-title>Tomate soundbox</v-toolbar-title>

            <v-progress-linear :active="playing" :value="progression" absolute bottom color="white"></v-progress-linear>

            <v-spacer></v-spacer>

            <v-btn text :disabled="!playing" @click="stopSound">
                <span class="mr-2">Stop</span>
                <v-icon>mdi-stop</v-icon>
            </v-btn>
        </v-app-bar>

        <v-main>
            <v-container>
                <v-row>
                    <v-col sm="6" md="4" v-for="s in sounds" :key="s.name">
                        <Sbutton v-bind="s" @playing="playSound" @stopped="endSound" @time="updateBar"></Sbutton>
                    </v-col>
                </v-row>
            </v-container>
        </v-main>
    </v-app>
</template>

<script>
import Sbutton from "./components/Sbutton.vue";
export default {
    name: "App",
    components: {
        Sbutton,
    },
    data: () => ({
        progression: 0,
        sounds: [
            {
                name: "PasMalNon",
                desc: "Pas mal non ?",
                playing: false,
            },
            {
                name: "uneImage",
                desc: "C'était une image",
                playing: false,
            },
            {
                name: "maisQueVoila",
                desc: "Heeeey",
                playing: false,
            },
            {
                name: "woodidip",
                desc: "Woodidip",
                playing: false,
            },
            {
                name: "chips",
                desc: "Chips",
                playing: false,
            },
            {
                name: "george",
                desc: "George",
                playing: false,
            },
        ],
    }),
    methods: {
        playSound(e) {
            const i = this.sounds.findIndex((s) => s.name == e);
            for (let j in this.sounds) {
                if (i != j && this.sounds[j].playing) {
                    this.stopSound();
                }
            }
            this.$set(this.sounds[i], "playing", true);
        },
        endSound(e) {
            const i = this.sounds.findIndex((s) => s.name == e);
            this.$set(this.sounds[i], "playing", false);
            this.progression = 0;
        },
        stopSound() {
            for (let s of this.sounds) {
                s.playing = false;
            }
            this.progression = 0;
        },
        updateBar(e) {
            this.progression = e;
        },
    },
    computed: {
        playing() {
            for (let s of this.sounds) {
                if (s.playing) {
                    return true;
                }
            }
            return false;
        },
    },
};
</script>