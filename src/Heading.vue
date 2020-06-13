<template>
    <component
        v-bind:is="`h${level}`"
        :style="headingStyles"
        :class="`headiing heading--${level}`">
            <slot></slot>
        </component>
</template>

<script>
export default {
    /**
     * Vue module «Heading» that draws heading in page with some params like 
     * it's level.
     * 
     * @property {number} level         — For correct heading level (h[level]).
     * @property {float} lineHeight     — Non important value, have fallback from original styles.
     * @property {boolean} calcOffset   — Trigger to start calculation compensation for test lines.
     * @property {array} calcMove        — Contain ratio that will be used in calculating additional offset...
     */
    name: 'Heading',
    props: {
        level: {
            type: Number,
            required: true
        },
        lineHeight: [Number, String],
        calcOffset: Boolean,
        calcMove: {
            type: Array,
            default(){
                return [2, 0]
            }
        }
    },
    data(){
        return {
            originalStyles: {
                lineHeight: null,
                marginTop: null,
                marginBottom: null
            },
            height: null,
            offsetTop: null,
            offsetBottom: null
        }
    },
    mounted(){
        this.getOriginalStyles();
        this.getHeight();
    },
    computed: {
        computedOffset() {
            /**
             * Computed caluclated offset. If calcOffset is set with true —
             * will calculate additional offset to make markup lines equal.
             * Depends on base style and require setted in CSS base lineheight
             * for HTML,BODY tags.
             * 
             * @return {object} computedOffset — contains top and bottom offset in float.
             */
            let offsetTop = this.originalStyles.marginTop;
            let offsetBottom = this.originalStyles.marginBottom;

            if(this.calcOffset){
                let fullHeight = offsetTop + this.height + offsetBottom;
                let moveRatio = this.calcMove[0] + this.calcMove[1];
                let lineSize = this.$root.lineSize ? this.$root.lineSize : parseFloat(window.getComputedStyle(document.documentElement, null).lineHeight);
                let restHeight = fullHeight % lineSize;
                if(restHeight) {
                    offsetTop += (lineSize - restHeight) / moveRatio * this.calcMove[0];
                    offsetBottom += (lineSize - restHeight) / moveRatio * this.calcMove[1];
                }
            }

            return {
                top: offsetTop,
                bottom: offsetBottom
            }
        },
        headingStyles() {
            /** 
             * Computed heading styles that contains conditions for diferent 
             * situations like compensation offset in good markup
             * 
             * @return {object} headingStyles — For heading styles
            */
            return {
                lineHeight: this.lineHeight ? this.lineHeight : this.originalStyles.lineHeight,
                marginTop: `${this.computedOffset.top}px`,
                marginBottom: `${this.computedOffset.bottom}px`,
            }
        }
    },
    methods: {
        getOriginalStyles(){
            /** 
             * Getting original styles of heading created. Usefull for cases
             * like if you dont have inline CSS code.
            */
            let styles = window.getComputedStyle(this.$el, null);

            this.originalStyles.lineHeight = parseFloat(styles.lineHeight) / parseFloat(styles.fontSize);
            this.originalStyles.marginTop = parseFloat(styles.marginTop);
            this.originalStyles.marginBottom = parseFloat(styles.marginBottom);
        },
        getHeight(){
            /**
             * Getting component element inner height.
             */
            this.height = parseFloat(window.getComputedStyle(this.$el, null).height);
        }
    }
}
</script>