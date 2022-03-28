<script>
export default {
  name: "YZoomImage",
  props: {
    src: String,
    size: {
      type: Object,
      default: () => ({ width: 200, height: 200 }),
    },
    getBigImage: {
      type: Function,
      default: (url) => url,
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
      bigImgUrl: "",
    };
  },
  methods: {
    handleMove(e) {
      let areaPos = {
        x: e.pageX - this.rect.left,
        y: e.pageY - this.rect.top,
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
      let rect = this.$refs.container.getBoundingClientRect();
      this.rect = rect;
      this.zoomx = rect.width / this.size.width;
      this.zoomy = rect.height / this.size.height;
      this.isEnter = true;
      // 返回大图片地址
      this.bigImgUrl = this.getBigImage(this.src);
    },
    handleLeave() {
      this.isEnter = false;
    },
  },
  render() {
    console.log(this.zoomx, this.zoomy);
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
                cursor: move;
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
                width:${this.rect.width * this.zoomx}px;
                height:${this.rect.height * this.zoomy}px;
                left:-${this.position.left * this.zoomx}px;
                top:-${this.position.top * this.zoomy}px; 
          `}
              src={this.bigImgUrl}
            />
          </div>
        )}
      </div>
    );
  },
};
</script>
