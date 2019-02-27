<template>
    <div class="kiwi-inputtool-emoji">
        <div class="kiwi-inputtool-emoji-palette" @mousedown.prevent @click.prevent>
            <div
                v-for="(text, eCode) in emojis"
                :key="eCode"
                :style="{'background-image':`url(${location}${eCode}.png)`}"
                :data-code="text + ' '"
                class="kiwi-inputtool-emoji-emoji"
                @click="onImgClick"
            />
        </div>
    </div>
</template>

<script>
'kiwi public';

import _ from 'lodash';
import state from '@/libs/state';
import * as Misc from '@/helpers/Misc';

export default {
    props: ['ircinput'],
    data: function data() {
        return {
            location: state.setting('emojiLocation'),
        };
    },
    computed: {
        emojis() {
            let list = {};
            let available = state.setting('emojis');
            _.each(available, (code, ascii) => {
                list[code] = ascii;
            });
            return list;
        },
    },
    methods: {
        onImgClick: function onImgClick(event) {
            let url = window.getComputedStyle(event.target, null)
                .getPropertyValue('background-image');

            url = Misc.extractURL(url);
            let code = event.target.dataset.code;
            this.ircinput.addImg(code, url);
        },
    },
};
</script>

<style>

.kiwi-controlinput-active-tool {
    overflow: hidden;
    border-radius: 10px 10px 0 0;
}

.kiwi-inputtool-emoji-palette {
    display: flex;
    justify-content: space-evenly;
    flex-direction: row;
    flex-wrap: wrap;
    padding: 5px 10px;
    border-top: 1px solid;
}

.kiwi-inputtool-emoji-emoji {
    display: flex;
    height: 30px;
    width: 30px;
    cursor: pointer;
    box-sizing: border-box;
    margin: 2px;
    background-size: contain;
    transform: rotate(0deg);
    transition: transform 0.2s;
    transition-timing-function: ease-out;
}

.kiwi-inputtool-emoji-emoji:hover {
    animation-name: rotateEmoji;
    animation-duration: 0.5s;
}

@keyframes rotateEmoji {
    0% { transform: rotate(0deg); }
    25% { transform: rotate(-15deg); }
    75% { transform: rotate(15deg); }
    100% { transform: rotate(0deg); }
}

</style>
