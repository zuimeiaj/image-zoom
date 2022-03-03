<script>
export default {
  name: "YZoomImage",
  props: {
    src: String,
    alt: String,
    size: {
      type: Object,
      default: () => ({ width: 150, height: 150 }),
    },
  },
  data() {
    return {
      isEnter: false,
      position: {
        left: 0,
        top: 0,
      },
      rect: {
        left: 0,
        top: 0,
        width: 0,
        height: 0,
      },
      zoom: 1,
    };
  },
  methods: {
    handleMove(e) {
      let areaPos = {
        x: e.clientX - this.rect.left,
        y: e.clientY - this.rect.top,
      };
      let x = areaPos.x - this.size.width / 2;
      let y = areaPos.y - this.size.height / 2;

      x = Math.max(x, 0);
      x = Math.min(x, this.rect.width - this.size.width);

      y = Math.max(y, 0);
      y = Math.min(y, this.rect.height - this.size.height);

      this.position = {
        left: x,
        top: y,
      };
    },
    handleEnter() {
      this.isEnter = true;
    },
    handleLeave() {
      this.isEnter = false;
    },
  },
  mounted() {
    let rect = this.$refs.container.getBoundingClientRect();
    this.rect = rect;
    this.zoom = rect.width / this.size.width;
  },
  render() {
    return (
      <div
        ref="container"
        style="position:relative;width:100%;height:100%"
        onMouseenter={this.handleEnter}
        onMouseleave={this.handleLeave}
        onMousemove={this.handleMove}
        class="yoo-image-zoom-container"
      >
        <img
          alt={this.alt}
          class="inner-image"
          style="width:100%;height:100%"
          src={this.src}
        />
        {this.isEnter && (
          <div
            style={`
                width:${this.size.width}px;
                height:${this.size.height}px;
                left:${this.position.left}px;
                top:${this.position.top}px;
                position: absolute;
                background: rgba(255, 255, 255, 0.5);
                cursor: none;
            `}
            class="moveable-box"
          ></div>
        )}
        {this.isEnter && (
          <div
            style={` 
                width: 100%;
                height: 100%;
                position: absolute;
                left: 0;
                top: 0;
                overflow: hidden;
                transform: translateX(101%);
    `}
            class="preview-box"
          >
            <img
              style={`
                position:absolute;
                width:${this.rect.width * this.zoom}px;
                height:${this.rect.height * this.zoom}px;
                left:-${this.position.left * this.zoom}px;
                top:-${this.position.top * this.zoom}px; 
          `}
              src={this.src}
            />
          </div>
        )}
      </div>
    );
  },
};
</script>
