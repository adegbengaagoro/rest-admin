<template>
  <div>
    
    <div class="preview py-2">
      <div class="thumbnail" v-if="_.isArray(model)">
        <b-draggable v-model="model" @input="update" class="row">
          <b-form-uploader-item class="col-4" :name="name" :id="`${id}_${k}`" :key="k"
          v-for="(v, k) in model" :field="field" :parent="parent" v-model="model[k]"
          @input="update"
          @remove="model.splice(k, 1) ; update()" 
          @add="model.splice(k + 1,0, null) ; update()" allow-add />
        </b-draggable>
      </div>
      <div v-else>
        <div class="row">
          <b-form-uploader-item class="col-12" :id="id" v-model="model" 
          @input="update" :name="name" :field="field" :parent="parent" @remove="model = null; update()" />
        </div>
      </div>
    </div>

    
  </div>
</template>
<script>
import bDraggable from "vuedraggable";
import bFormUploaderItem from "./FormUploaderItem";
export default {
  name: "b-form-uploader",
  components: {
    bDraggable,
    bFormUploaderItem
  },
  props: {
    value: {},
    field: {},
    id: {},
    parent: {},
    name: {}
  },
  data() {
    return {
      model: this.value,
      file: null,
      current: this.value
    };
  },
  watch: {
    value(val){
      this.model = val
    }
  },
  computed: {
    tag() {
      return this.field.type == "image" ? "img" : this.field.type;
    }
  },
  methods: {
    update(){
      this.$emit('input', this.model)
    }
  }
};
</script>

<style>
.current {
  border: 1px solid #c30;
}
</style>

