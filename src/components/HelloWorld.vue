<template>
  <div id="app">
    <dashboard
      ref="dash"
      :uppy="uppy"
      :props="{
        metaFields: [
          { id: 'name', name: 'Name', placeholder: 'file name' },
          {
            id: 'caption',
            name: 'Caption',
            placeholder: 'describe what the image is about',
          },
        ],
      }"
      :plugins="['Webcam', 'ImageEditor']"
    />
  </div>
</template>

<script>
import { Dashboard } from "@uppy/vue";

import "@uppy/core/dist/style.css";
import "@uppy/dashboard/dist/style.css";
import "@uppy/webcam/dist/style.css";
import "@uppy/image-editor/dist/style.css";

import Uppy from "@uppy/core";
import Webcam from "@uppy/webcam";
import XHRUpload from "@uppy/xhr-upload";
import ImageEditor from "@uppy/image-editor";
const uppy = new Uppy({
  metaFields: [
    { id: "name", name: "Name", placeholder: "file name" },
    {
      id: "caption",
      name: "Caption",
      placeholder: "describe what the image is about",
    },
  ],
});

uppy.use(XHRUpload, {
  endpoint: "http://localhost:3000/file?path=uppy",
  fieldName: "file",
});
uppy.use(Webcam);
uppy.use(ImageEditor, {
  quality: 0.8,
});
export default {
  name: "App",
  components: {
    Dashboard,
  },
  data: () => {
    return {
      uppy,
    };
  },
  computed: {
    // uppy: () =>
    //   new Uppy().use(XHRUpload, {
    //     endpoint: "http://localhost:3000/files",
    //     fieldName: "file",
    //   }),
  },
  beforeUnmount() {
    this.uppy.close();
  },
};
</script>