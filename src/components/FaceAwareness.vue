<template>
<div class="train">
  <h1>{{ detected_e }}</h1>
  <template v-if="mode == 'train'">
    <h3>Take pictures that define your different moods in the dropdown</h3>
  </template>
  <template v-else>
    <h3>Take a picture to let us know how you feel about our service</h3>
  </template>
  <select id="use_case" v-on:change="changeOption()">
    <option value="train">Train</option>
    <option value="test">Test</option>
  </select>
  <select id="emotion_options" v-if="mode == 'train'">
    <template v-for="(emotion, index) in emotions">
      <option :key="index" :value="index">{{emotion}}</option>
    </template>
  </select>
  <Camera></Camera>
  <template v-if="mode == 'train'">
    <button v-on:click="trainModel()">Train</button>
  </template>
  <template v-else>
    <button v-on:click="getEmotion()">Get</button>
  </template>
</div>
</template>

<style>
.FaceAwareness {
  display: flex;
  margin: auto 10vw;
  width: 100%;
}

.train {
  width: auto;
  margin: auto 10vw;
}

.train>#use_case,
.train>#emotion_options {
  height: 40px;
  width: 80px;
  text-decoration: none;
  background: white;
  cursor: pointer;
  margin: 10px 10px;
  outline: none;
  border: solid 2px #c1c1c1;
}

.train button {
  margin: 20px 10px;
  outline: none;
  cursor: pointer;
  width: 100px;
  height: 100px;
  display: inline-block;
  text-decoration: none;
  background: white;
  color: #EF6C00;
  font-size: 20px;
  line-height: 100px;
  border-radius: 50%;
  text-align: center;
  vertical-align: middle;
  overflow: hidden;
  box-shadow: inset 0px 3px 0 rgba(255, 255, 255, 0.3), 0 3px 3px rgba(0, 0, 0, 0.3);
  font-weight: bold;
  border-bottom: solid 3px #c1c1c1;
  text-shadow: 1px 1px 1px rgba(255, 255, 255, 0.65);
  transition: .4s;
}

.train button:hover {
  -ms-transform: translateY(1px);
  -webkit-transform: translateY(1px);
  transform: translateY(1px);
  box-shadow: 0 0 2px #EF6C00;
  border-bottom: none;
}

.train h1 {
  color: #EF6C00;
}

.train h3 {
  border: 1px solid #c1c1c1;
  border-radius: .5em;
  padding: 10px;
}
</style>

<script>
// @ is an alias to /src
import Camera from "@/components/Camera.vue";
import * as tf from '@tensorflow/tfjs';
import * as mobilenetModule from '@tensorflow-models/mobilenet';
import * as knnClassifier from '@tensorflow-models/knn-classifier';
import axios from 'axios';
import FaceModel from './FaceModel.vue'

export default {
  name: "FaceAwareness",
  components: {
    Camera,
    FaceModel
  },
  data: function() {
    return {
      emotions: ['angry', 'neutral', 'happy'],
      classifier: null,
      mobilenet: null,
      class: null,
      detected_e: null,
      mode: 'train',
    }
  },
  mounted: function() {
    this.init();
  },
  methods: {
    async init() {
      // load the load mobilenet and create a KnnClassifier
      this.classifier = knnClassifier.create();
      this.mobilenet = await mobilenetModule.load();
    },
    trainModel() {
      let selected = document.getElementById("emotion_options");
      this.class = selected.options[selected.selectedIndex].value;
      this.addExample();
      console.log(this.class);
      if (this.class == 0) {
        alert('trained angry');
      } else if (this.class == 1) {
        alert('trained neutral');
      } else {
        alert('trained happy');
      }
    },
    addExample() {
      const img = tf.fromPixels(this.$children[0].webcam.webcamElement);
      const logits = this.mobilenet.infer(img, 'conv_preds');
      this.classifier.addExample(logits, parseInt(this.class));
    },
    async getEmotion() {
      const img = tf.fromPixels(this.$children[0].webcam.webcamElement);
      const logits = this.mobilenet.infer(img, 'conv_preds');
      const pred = await this.classifier.predictClass(logits);
      this.detected_e = this.emotions[pred.classIndex];
      this.registerEmotion();
    },
    changeOption() {
      const selected = document.getElementById("use_case");
      this.mode = selected.options[selected.selectedIndex].value;
    },
    registerEmotion() {
      axios.post('http://localhost:3128/callback', {
        'emotion': this.detected_e,
        // headers: {
        //   Authorization: 'Bearer ${token}',
        //   'Content-Type': 'application/json'
        // }
      }).then(() => {
        console.log(this.detected_e);
      });
    }
  }
};
</script>
