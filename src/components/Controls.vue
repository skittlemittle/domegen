<template>
  <div ref="controlPanel" class="control-panel">
    <div class="input-group">
      <h1 @click="toggleCircle" :class="{ 'deactivated-segment': !showCircle }">Circle</h1>
      <h2>Radius</h2>
      <input type="number" :value="radius" @input="updateRadius" />
      <h2>Zoom </h2>
      <input type="number" :value="zoom" @input="updateZoom" />
    </div>

    <div class="input-group">
      <h1 @click="toggleEllipse" :class="{ 'deactivated-segment': !showEllipse }">Ellipse</h1>
      <h2>Height</h2>
      <input type="number" :value="height" @input="updateEllipseHeight" />
      <h2>Width</h2>
      <input type="number" :value="width" @input="updateEllipseWidth" />
    </div>

    <h1 @click="toggleGrid" id="last-input" :class="{ 'deactivated-segment': !showGrid }">Show Grid</h1>
  </div>
</template>

<script>
import { defineComponent, ref } from 'vue';

export default defineComponent({
  props: {
    radius: {
      type: Number,
      required: true
    },
    zoom: {
      type: Number,
      required: true
    },
    height: {
      type: Number,
      required: true
    },
    width: {
      type: Number,
      required: true
    },
    showEllipse: {
      type: Boolean,
      required: true
    },
    showCircle: {
      type: Boolean,
      required: true
    },
    showGrid: {
      type: Boolean,
      required: true
    }
  },
  emits: ['update:radius', 'update:zoom', 'update:height', 'update:width', 'update:showEllipse', 'update:showCircle', 'update:showGrid'],
  setup(props, { emit }) {
    const updateNumericControl = (update, val) => {
      if (!isNaN(Number(val)) && val !== "")
        emit(update, val)
    }

    const updateRadius = (event) => {
      updateNumericControl('update:radius', event.target.value)
    };

    const updateZoom = (event) => {
      updateNumericControl('update:zoom', Math.max(event.target.value, 10))
    }

    const updateEllipseHeight = (event) => {
      updateNumericControl('update:height', event.target.value)
    }

    const updateEllipseWidth = (event) => {
      updateNumericControl('update:width', event.target.value)
    }

    const toggleEllipse = () => emit('update:showEllipse', !props.showEllipse)
    const toggleCircle = () => emit('update:showCircle', !props.showCircle)
    const toggleGrid = () => emit('update:showGrid', !props.showGrid)

    const controlPanel = ref(null)

    return {
      updateRadius,
      updateZoom,
      updateEllipseHeight,
      updateEllipseWidth,
      toggleEllipse,
      toggleCircle,
      toggleGrid,
      controlPanel,
    };
  }
});
</script>

<style scoped>
.control-panel {
  padding: 0 1rem;
  max-width: fit-content;
}

h1 {
  margin-top: 0.5rem;
  color: var(--color-heading);
}

@media (max-width: 50rem) {
  .control-panel {
    display: flex;
    flex-direction: row;
    padding: 0 0 0.5rem 0.5rem;
  }

  .input-group {
    margin-right: 1rem;
  }

  h1 {
    margin: 0
  }
}

@media (max-height: 50rem) {
  .control-panel {
    overflow-x: scroll;
  }

  #last-input {
    margin-bottom: 5rem;
  }

}

@media (max-width: 30rem) {
  input {
    max-width: 5rem;
  }
}

input {
  font-size: max(16px, 1rem);
  background-color: var(--color-background);
  color: var(--color-text);
  border: 2px solid var(--color-border);
}

input:focus {
  outline: none;
  border: 2px solid var(--color-border-hover)
}


.deactivated-segment {
  text-decoration: line-through;
}
</style>