<script>
/* global Blotter */
export default {
  name: "VueBlotter",
  props: {
    // Controls
    autoplay: {
      type: Boolean,
      default: true
    },
    checkInViewport: {
      type: Boolean,
      default: true
    },
    // Text
    text: {
      type: String,
      default: "Blotter"
    },
    family: {
      type: String,
      default: "serif"
    },
    size: {
      type: Number,
      default: 120
    },
    fill: {
      type: String,
      default: "#171717"
    },
    textStyle: {
      type: String,
      default: "normal"
    },
    weight: {
      type: Number,
      default: 400
    },
    padding: {
      type: Number,
      default: 0
    },
    paddingTop: {
      type: Number,
      default: 0
    },
    paddingRight: {
      type: Number,
      default: 0
    },
    paddingBottom: {
      type: Number,
      default: 0
    },
    paddingLeft: {
      type: Number,
      default: 0
    },
    // Material
    materialType: {
      type: String,
      required: true,
    },
    uniforms: {
      type: Object,
      default: () => {}
    }
  },
  data: () => ({
    blotter: {},
    scope: {},
    intersecting: null
  }),
  mounted() {
    // Instantiate global instance of Blotter class
    this.initBlotter();
    
    if (this.checkInViewport) {
      this.checkIntersection();
    }
  },
  beforeDestroy() {
    // Stop updating the blotter instance if component is destroyed
    this.blotter.stop();
  },
  watch: {
    intersecting(value) {
      if (value) {
        this.scope.play();
      } else {
        this.scope.pause();
      }
    }
  },
  methods: {
    initBlotter() {
      if (!this.materialType) {
        throw new Error('Material type prop is required. Refer to https://github.com/arapl3y/vue-blotter#configuration-options')
      }

      // Set text values according to props
      const text = this.setBlotterText({
        text: this.text,
        family: this.family,
        size: this.size,
        fill: this.fill,
        textStyle: this.textStyle,
        weight: this.weight,
        padding: this.padding,
        paddingTop: this.paddingTop,
        paddingRight: this.paddingRight,
        paddingBottom: this.paddingBottom,
        paddingLeft: this.paddingLeft
      });

      // Set material according to props
      const material = this.setBlotterMaterial(this.materialType);

      // Check if uniforms object exist and contains properties
      if (this.uniforms && Object.keys(this.uniforms).length !== 0) {
        this.setBlotterUniforms(material, this.uniforms);
      }

      // Init blotter and insert into root DOM node as canvas element
      this.blotter = this.renderBlotter(material, text);

      this.setBlotterScope(this.blotter, text, this.$el);
    },
    setBlotterText({ text, textStyle, ...args }) {
      return new Blotter.Text(text, {
        style: textStyle,
        ...args
      });
    },
    setBlotterMaterial(type) {
      return new Blotter[type]();
    },
    setBlotterUniforms(material, uniforms) {
      for (const key in uniforms) {
        material.uniforms[key].value = uniforms[key];
      }
    },
    renderBlotter(material, text) {
      return new Blotter(material, {
        autoplay: this.autoplay,
        texts: text
      });
    },
    setBlotterScope(blotter, text, el) {
      this.scope = blotter.forText(text);
      // Append to slot element
      this.scope.appendTo(el);
    },
    pause(scope) {
      scope.pause();
    },
    play(scope) {
      scope.play();
    },
    checkIntersection() {
      const observer = new IntersectionObserver(
        entries => {
          entries.forEach(entry => {
            if (entry.isIntersecting) {
              this.intersecting = true;
            } else {
              this.intersecting = false;
            }
          });
        },
        { threshold: 0.3 }
      );

      observer.observe(this.$el);
    }
  },
  render() {
    return this.$scopedSlots.default({
      blotterScope: this.scope,
      intersecting: this.intersecting
    });
  }
};
</script>