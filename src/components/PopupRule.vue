<template>
  <div class="wrapper-popup" v-bind:class="getClassPopup">
    <div class="rule">
      <h3>Luat Choi</h3>
      <p>
        Lorem ipsum dolor sit amet consectetur adipisicing elit. Quibusdam
        tenetur illum repudiandae suscipit explicabo modi culpa placeat
        provident aliquid, dolore quam aperiam soluta nam reiciendis dolorem.
        Error quidem perferendis molestias.
      </p>
      <button class="confirm" v-on:click="closePopup">Ok</button>
    </div>
  </div>
</template>

<script>
export default {
  name: "popup-rule",
  props: { isOpenPopup: { type: Boolean, default: false } },
  data() {
    return {};
  },
  computed: {
    getClassPopup() {
      return {
        "open-popup": this.isOpenPopup,
      };
    },
  },
  methods: {
    closePopup() {
      this.$emit("handleClosePopup");
    },
  },
};
</script>

<style>
.wrapper-popup {
  position: fixed;
  top: 0;
  bottom: 0;
  right: 0;
  left: 0;
  background: rgba(0, 0, 0, 0.4);
  opacity: 0;
  visibility: hidden;
  transition: all 0.3s ease;
}
.wrapper-popup.open-popup {
  opacity: 1;
  visibility: visible;
  transition: all 0.3s ease;
}
.open-popup .rule {
  transform: translate(-50%, -50%) scale(1);
}
.rule {
  top: 50%;
  left: 50%;
  width: 350px;
  padding: 20px;
  background: #fff;
  position: absolute;
  transform: translate(-50%, -50%) scale(1.2);
}
.rule h3 {
  margin-bottom: 10px;
}
.rule .confirm {
  cursor: pointer;
  margin-top: 20px;
  border: 1px solid #000;
  transition: all 0.3s ease;
  padding: 10px 20px;
  border-radius: 10px;
}
.rule .confirm:hover {
  background: #000;
  color: #fff;
}
</style>
