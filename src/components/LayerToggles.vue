<template>
  <div class="toggles">
    <div v-for="(state, index) in toggles" :key="state.name" class="toggleItem">
      <label class="label-container"
        >{{ state.label }}
        <input
          :id="state.name"
          :value="state.label"
          name="layer"
          type="checkbox"
          checked
          @click="handleToggle(index, $event)" />
        <span class="checkmark"></span></label
      ><br />
    </div>
  </div>
</template>

<script>
export default {
  props: {
    map: {
      type: Object,
      default: null,
    },
    toggles: {
      type: Array,
      default: null,
    },
  },
  methods: {
    handleToggle(index, event) {
      const visibility = event.target.checked;
      if (visibility === true && this.map !== null) {
        this.toggles[index].layers.forEach((layer) => {
          this.map.setLayoutProperty(layer, 'visibility', 'visible');
        });
      } else if (visibility === false && this.map !== null) {
        this.toggles[index].layers.forEach((layer) => {
          this.map.setLayoutProperty(layer, 'visibility', 'none');
        });
      } else if (this.map === null) {
        console.warn('The map has not finished loading yet');
      } else {
        console.error(
          'The visiblity property on the payload is not a boolean value'
        );
      }
    },
  },
};
</script>

<style lang="scss">
.toggles {
  background-color: rgba(107, 105, 101, 0.9);
  box-sizing: border-box;
  padding: 1em;
  position: absolute;
  top: 1em;
  left: 1em;
  max-width: 400px;
  z-index: 2;
  @media (max-width: 768px) {
    width: 100%;
    max-width: unset;
    left: 0;
    top: 0;
  }
  .toggleItem {
    display: flex;
    margin: 1em 0;
  }
  label,
  span {
    color: white;
    font-size: 18px;
    font-family: 'Blender';
    font-weight: 500;
    text-transform: uppercase;
  }
  .label-container {
    display: block;
    position: relative;
    padding-left: 35px;
    cursor: pointer;
    -webkit-user-select: none;
    -moz-user-select: none;
    -ms-user-select: none;
    user-select: none;
    input {
      position: absolute;
      opacity: 0;
      cursor: pointer;
      height: 0;
      width: 0;
    }
    .checkmark {
      box-sizing: border-box;
      position: absolute;
      top: -3px;
      left: 0;
      height: 24px;
      width: 24px;
      background-color: #6b6965;
      border: 2px solid white;
      &:after {
        content: '';
        position: absolute;
        display: none;
      }
    }
  }
  .label-container:hover input ~ .checkmark {
    background-color: #6b6965;
  }
  .label-container input:checked ~ .checkmark {
    background-color: #0e9797;
  }
  .label-container input:checked ~ .checkmark:after {
    display: block;
  }
  .label-container .checkmark:after {
    top: 9px;
    left: 9px;
    width: 8px;
    height: 8px;
    border-radius: 50%;
  }
}
</style>
