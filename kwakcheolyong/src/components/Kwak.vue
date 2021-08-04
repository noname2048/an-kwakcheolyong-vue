<template>
    <h2>곽철용짤생성기 - 클론</h2>
    <select :value="imageIndex" @input="onImageChanged($event.target.value)">
        <option v-for="img in images" :value="img.id" :key="img.id">
            {{ img.name }}
        </option>
    </select>
    <input type="textarea" :value="option.txt" @input="onValueChanged('text', $event.target.value)"/>
    <select :value="option.fontFamily" @input="onValueChanged('fontFamily', $event.target.value)">
        <option value="Nanum Gothic">Gothic</option>
        <option value="Gulim">Gulim</option>
        <option value="Arial">Arial</option>
        <option value="Helvetica">Helvetica</option>
    </select>
    <select :value="option.fontSize" @input="onValueChanged('fontSize', $event.target.value)">
        <option :value="20">20</option>
        <option :value="25">25</option>
        <option :value="30">30</option>
        <option :value="35">35</option>
    </select>
    <select :value="option.textBorder" @input="onValueChanged('textBorder', $event)">
        <option value="transparent" lable="없음">없음</option>
        <option value="black" lable="검정">검정</option>
        <option value="white" lable="흰색">흰색</option>
    </select>

    <canvas id="canvas" ref="canvas" :width="1000" :height="427">
        이 브라우저는 HTML5의 canvas 요소를 지원하지 않습니다
    </canvas>

    <button @click="downloadCanvas" id="download">다운로드</button>
</template>

<script>
import images from "@/images";

export default {
    data() {
        return {
            imageIndex: 0,
            option: {
                fontFamily: 'Gulim',
                fontSize: 30,
                fontColor: '#FFFFFF',
                fontWeight: 'normal',
                text: '',
                textBorder: "black",
            },
        };
    },

    computed: {
        images() {
            return images;
        },
    },

    mounted() {
        this.updateCanvas();
    },

    methods: {
        updateCanvas() {
            if (!this.$refs.canvas) return;
            this.updateCanvasImage();
        },   
        updateCanvasImage() {
            const { canvas } = this.$refs;
            const ctx = canvas.getContext("2d");
            const img = new Image();
            img.src = this.images[this.imageIndex].src;
            img.onload = () => {
                ctx.drawImage(img, 0, 0);
                this.updateCanvasText();
            };
        },
        onImageChanged(value) {
            this.imageIndex = value;
            this.updateCanvas();
        },
        onValueChanged(key, value) {
            this.option[key] = value;
            this.updateCanvas();
        },
        updateCanvasText() {
            const { canvas } = this.$refs;
            const ctx = canvas.getContext("2d");
            const { text, fontFamily, fontSize, fontColor, fontWeight, textBorder } = this.option;

            ctx.textAlign = "center";
            ctx.textBaseline = "middle";
            ctx.font = `${fontWeight} ${fontSize}px ${fontFamily}`;

            const lines = text.split("\n");
            lines.forEach((line, index) => {
                ctx.linewidth = 5;
                ctx.strokeStyle = `${textBorder}`;
                ctx.strokeText(
                    line,
                    canvas.width / 2,
                    canvas.height - fontSize * (lines.length - index) * 1.5,
                );

                ctx.fillStyle = fontColor;
                ctx.fillText(
                    line,
                    canvas.width / 2,
                    canvas.height - fontSize * (lines.length - index) * 1.5,
                );
            });
        },
        downloadCanvas(){
            const url = this.$refs.canvas.toDataURL("image/png");
            const link = document.createElement("a");
            link.href = url;
            link.setAttribute("download", `${this.images[this.imageIndex].name}.png`);
            document.body.appendChild(link);
            link.click();
            document.body.removeChild(link);
        },
    },
}

</script>

<style lang="scss">

</style>

