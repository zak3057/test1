<template>
  <div>
    <md-editor
      v-model="text"
      language="en-US"
      :toolbars="toolbars"
      ref="editorRef"
    >
      <template #defToolbars>
        <dropdown-toolbar
          :visible="isVisibleDropdown"
          :on-change="onChangeDropdown"
        >
          <template #trigger>
            <svg class="md-editor-icon" aria-hidden="true">
              <use xlink:href="#md-editor-icon-image"></use>
            </svg>
          </template>
          <template #overlay>
            <div class="md-dropdown-overlay">
              <ul class="md-editor-menu">
                <li class="md-editor-menu-item" @click="onClickInsertImage">
                  insert image
                </li>
                <li
                  class="md-editor-menu-item"
                  @click="onClickInsertDefaultImage"
                >
                  insert default image
                </li>
              </ul>
            </div>
          </template>
        </dropdown-toolbar>
      </template>
    </md-editor>

    <hr />

    <md-editor v-model="text" preview-only />
  </div>
</template>

<script>
import { ref } from "vue";
import MdEditor from "md-editor-v3";

MdEditor.config({
  markedRenderer(renderer) {
    renderer.link = (href, title, text) => {
      return `<a href="${href}" title="${title}" target="_blank">${text}</a>`;
    };
    return renderer;
  },
});

const DropdownToolbar = MdEditor.DropdownToolbar;

export default {
  components: {
    MdEditor,
    DropdownToolbar,
  },
  setup() {
    const text = ref("");
    const toolbars = [
      "bold",
      "italic",
      "-",
      "title",
      "strikeThrough",
      "-",
      "link",
      0,
    ];
    const editorRef = ref();
    const isVisibleDropdown = ref(false);

    const onChangeDropdown = (x) => {
      isVisibleDropdown.value = x;
    };

    const onClickInsertImage = () => {
      editorRef.value?.insert(() => {
        return {
          targetValue: "![](url)",
          select: true,
          deviationStart: 0,
          deviationEnd: 0,
        };
      });
    };

    const onClickInsertDefaultImage = () => {
      editorRef.value?.insert(() => {
        return {
          targetValue:
            "![](https://helpx.adobe.com/content/dam/help/en/photoshop/using/quick-actions/remove-background-before-qa1.png)",
          select: true,
          deviationStart: 0,
          deviationEnd: 0,
        };
      });
    };

    return {
      text,
      toolbars,
      isVisibleDropdown,
      onChangeDropdown,
      onClickInsertImage,
      onClickInsertDefaultImage,
      editorRef,
    };
  },
};
</script>

<style deep>
.md-editor-checkbox {
  cursor: pointer;
  width: 12px;
  height: 12px;
  border: 1px solid var(--md-border-color);
  background-color: var(--md-bk-color-outstand);
  border-radius: 2px;
  line-height: 1;
  text-align: center;
}
.md-editor-checkbox:after {
  content: "";
  font-weight: 700;
}
.md-editor-checkbox-checked:after {
  content: "\2713";
}
.md-editor-divider {
  position: relative;
  display: inline-block;
  width: 1px;
  top: 0.1em;
  height: 0.9em;
  margin: 0 8px;
  background-color: var(--md-border-color);
}
.md-editor-dropdown {
  overflow: hidden;
  box-sizing: border-box;
  position: absolute;
  transition: all 0.3s;
  opacity: 1;
  z-index: 10000;
  background-color: var(--md-bk-color);
}
.md-editor-dropdown-hidden {
  opacity: 0;
  z-index: -10000;
}
.md-editor-dropdown-overlay {
  margin-top: 6px;
}
.md-editor-modal-mask {
  position: fixed;
  top: 0;
  right: 0;
  bottom: 0;
  left: 0;
  z-index: 20000;
  height: 100%;
  background-color: var(--md-modal-mask);
}
.md-editor-modal {
  display: block;
  background-color: var(--md-bk-color);
  border-radius: 3px;
  border: 1px solid var(--md-border-color);
  position: fixed;
  z-index: 20001;
  flex-direction: column;
}
.md-editor-modal-header {
  cursor: grab;
  display: flex;
  justify-content: space-between;
  padding: 10px 24px;
  color: var(--md-color);
  font-weight: 600;
  font-size: 16px;
  line-height: 22px;
  word-wrap: break-word;
  user-select: none;
  border-bottom: 1px solid var(--md-border-color);
  position: relative;
}
.md-editor-modal-body {
  padding: 24px;
  font-size: 14px;
  word-wrap: break-word;
  height: calc(100% - 61px);
  box-sizing: border-box;
}
.md-editor-modal .md-editor-modal-func {
  position: absolute;
  top: 10px;
  right: 10px;
}
.md-editor-modal .md-editor-modal-func .md-editor-modal-adjust,
.md-editor-modal .md-editor-modal-func .md-editor-modal-close {
  cursor: pointer;
  width: 24px;
  height: 24px;
  line-height: 24px;
  text-align: center;
  display: inline-block;
}
.md-editor-modal .md-editor-modal-func .md-editor-modal-adjust {
  padding-right: 10px;
}
.animation {
  animation-duration: 0.15s;
  animation-fill-mode: forwards;
}
.md-editor-clip {
  position: relative;
  display: flex;
  height: calc(100% - 32px);
}
.md-editor-clip-main,
.md-editor-clip-preview {
  width: 50%;
  height: 100%;
  border: 1px solid var(--md-border-color);
}
.md-editor-clip-main {
  margin-right: 1em;
}
.md-editor-clip-main .md-editor-clip-cropper {
  position: relative;
  width: 100%;
  height: 100%;
}
.md-editor-clip-main .md-editor-clip-cropper .md-editor-clip-delete {
  position: absolute;
  top: 0;
  right: 0;
  font-size: 0;
  background-color: var(--md-bk-color-outstand);
  border-bottom-left-radius: 4px;
  color: var(--md-color);
  cursor: pointer;
}
.md-editor-clip-main .md-editor-clip-upload {
  display: flex;
  align-items: center;
  justify-content: center;
  width: 100%;
  height: 100%;
  cursor: pointer;
}
.md-editor-clip-main .md-editor-clip-upload .md-editor-icon {
  width: auto;
  height: 40px;
}
.md-editor-clip-preview-target {
  width: 100%;
  height: 100%;
  overflow: hidden;
}
@media (max-width: 688px) {
  .md-editor-modal-clip .md-editor-modal {
    max-width: calc(100% - 20px);
    max-height: calc(100% - 20px);
    margin: 10px;
    left: 0 !important;
  }
  .md-editor-modal-clip .md-editor-clip {
    flex-direction: column;
  }
  .md-editor-modal-clip .md-editor-clip-main,
  .md-editor-modal-clip .md-editor-clip-preview {
    width: 100%;
    height: 0;
    flex: 1;
  }
  .md-editor-modal-clip .md-editor-clip-main {
    margin-bottom: 1em;
  }
}
.md-editor {
  --md-color: #222;
  --md-hover-color: #000;
  --md-bk-color: #fff;
  --md-bk-color-outstand: #f6f6f6;
  --md-bk-hover-color: #f5f7fa;
  --md-border-color: #e6e6e6;
  --md-border-hover-color: #b9b9b9;
  --md-border-active-color: #999;
  --md-modal-mask: #00000073;
  --md-scrollbar-bg-color: #e2e2e2;
  --md-scrollbar-thumb-color: #0000004d;
  --md-scrollbar-thumb-hover-color: #00000059;
  --md-scrollbar-thumb-avtive-color: #00000061;
  width: 100%;
  height: 425px;
  position: relative;
  box-sizing: border-box;
  border: 1px solid var(--md-border-color);
  display: flex;
  flex-direction: column;
  overflow: hidden;
  color: var(--md-color);
  background-color: var(--md-bk-color);
  font-family: -apple-system, BlinkMacSystemFont, Segoe UI Variable, Segoe UI,
    system-ui, ui-sans-serif, Helvetica, Arial, sans-serif, "Apple Color Emoji",
    "Segoe UI Emoji";
}
.md-editor ::-webkit-scrollbar {
  width: 6px;
  height: 6px;
}
.md-editor ::-webkit-scrollbar-corner,
.md-editor ::-webkit-scrollbar-track {
  background-color: var(--md-scrollbar-bg-color);
}
.md-editor ::-webkit-scrollbar-thumb {
  border-radius: 2px;
  background-color: var(--md-scrollbar-thumb-color);
}
.md-editor ::-webkit-scrollbar-button:vertical {
  display: none;
}
.md-editor ::-webkit-scrollbar-thumb:vertical:hover {
  background-color: var(--md-scrollbar-thumb-hover-color);
}
.md-editor ::-webkit-scrollbar-thumb:vertical:active {
  background-color: var(--md-scrollbar-thumb-avtive-color);
}
.md-editor-icon {
  width: 24px;
  height: 24px;
  fill: currentColor;
  overflow: hidden;
}
.md-editor-menu {
  margin: 0;
  padding: 0;
  border-radius: 3px;
  border: 1px solid var(--md-border-color);
  background-color: inherit;
}
.md-editor-menu-item {
  list-style: none;
  font-size: 12px;
  color: var(--md-color);
  padding: 4px 10px;
  cursor: pointer;
  line-height: 16px;
}
.md-editor-menu-item:first-of-type {
  padding-top: 8px;
}
.md-editor-menu-item:last-of-type {
  padding-bottom: 8px;
}
.md-editor-menu-item:hover {
  background-color: var(--md-bk-hover-color);
}

.md-editor-toolbar-wrapper {
  overflow-x: auto;
  overflow-y: hidden;
  scrollbar-width: none;
  height: 35px;
  flex-shrink: 0;
  padding: 4px;
  border-bottom: 1px solid var(--md-border-color);
}
.md-editor-toolbar-wrapper::-webkit-scrollbar {
  height: 0 !important;
}
.md-editor-toolbar-wrapper .md-editor-toolbar {
  min-width: 850px;
  height: 100%;
  display: flex;
  justify-content: space-between;
  align-items: center;
  box-sizing: content-box;
}
.md-editor-toolbar-wrapper .md-editor-toolbar-item {
  height: 24px;
  display: inline-block;
  padding: 0 4px;
  transition: all 0.3s;
  border-radius: 0;
  cursor: pointer;
  list-style: none;
  user-select: none;
}
.md-editor-toolbar-wrapper .md-editor-toolbar-item:hover {
  border-radius: 3px;
  background-color: var(--md-bk-color-outstand);
}
.md-editor-toolbar-wrapper .md-editor-toolbar-left,
.md-editor-toolbar-wrapper .md-editor-toolbar-right {
  padding: 1px 0;
  display: flex;
  align-items: center;
}
.md-editor-content {
  display: flex;
  flex: 1;
  height: 0;
  flex-shrink: 0;
}
.md-editor-content .md-editor-input-wrapper {
  flex: 1;
  box-sizing: border-box;
}
.md-editor-content .md-editor-input-wrapper textarea {
  width: 100%;
  height: 100%;
  padding: 10px 20px;
  box-sizing: border-box;
  font-size: 1em;
  font-family: -apple-system, Consolas, BlinkMacSystemFont, Segoe UI, Helvetica,
    Arial, sans-serif, "Apple Color Emoji", "Segoe UI Emoji", Segoe UI Symbol;
  resize: none;
  border: none;
  outline: none;
  overflow-y: scroll;
  color: inherit;
  background-color: inherit;
  line-height: 1.4;
}
.md-editor-content .md-editor-input-wrapper textarea:disabled {
  cursor: not-allowed;
}
.md-editor-content .md-editor-input-wrapper textarea.textarea-only {
  overflow-y: auto;
}
.md-editor-content .md-editor-preview-wrapper {
  position: relative;
  flex: 1;
  box-sizing: border-box;
  overflow: auto;
}
.md-editor-content .md-editor-preview,
.md-editor-content .md-editor-html {
  padding: 10px 20px;
  font-size: 16px;
  position: relative;
  word-break: break-all;
}
.md-editor-footer {
  height: 24px;
  flex-shrink: 0;
  font-size: 12px;
  color: var(--md-color);
  border-top: 1px solid var(--md-border-color);
  display: flex;
  justify-content: space-between;
}
.md-editor-footer-item {
  display: inline-flex;
  align-items: center;
  height: 100%;
  padding: 0 10px;
}
.md-editor-footer-label {
  padding-right: 5px;
  line-height: 1;
}
.md-editor-form-item {
  margin-bottom: 14px;
  text-align: center;
}
.md-editor-form-item:last-of-type {
  margin-bottom: 0;
}
.md-editor-label {
  font-size: 14px;
  color: var(--md-color);
  width: 80px;
  text-align: center;
  display: inline-block;
}
.md-editor-input {
  border-radius: 4px;
  padding: 4px 11px;
  color: var(--md-color);
  font-size: 14px;
  line-height: 1.5715;
  background-color: var(--md-bk-color);
  background-image: none;
  border: 1px solid var(--md-border-color);
  transition: all 0.2s;
}
.md-editor-input:focus,
.md-editor-input:hover {
  border-color: var(--md-border-hover-color);
  outline: 0;
}
.md-editor-input:focus {
  border-color: var(--md-border-active-color);
}
.md-editor-btn {
  font-weight: 400;
  text-align: center;
  vertical-align: middle;
  cursor: pointer;
  border: 1px solid var(--md-border-color);
  white-space: nowrap;
  user-select: none;
  height: 32px;
  padding: 0 15px;
  font-size: 14px;
  border-radius: 4px;
  transition: all 0.2s linear;
  color: var(--md-color);
  background-color: var(--md-bk-color);
  border-color: var(--md-border-color);
  margin-left: 10px;
}
.md-editor-btn:first-of-type {
  margin-left: 0;
}
.md-editor-btn:hover {
  color: var(--md-hover-color);
  background-color: var(--md-bk-color);
  border-color: var(--md-border-hover-color);
}
.md-editor-btn-row {
  width: 100%;
}
.md-editor-previewOnly {
  border: none;
  height: auto;
}
.md-editor-previewOnly .md-editor-content {
  height: 100%;
}
.md-editor-previewOnly .md-editor-preview {
  padding: 0;
}
.default-theme ::-webkit-scrollbar {
  width: 6px;
  height: 6px;
}
.default-theme ::-webkit-scrollbar-corner,
.default-theme ::-webkit-scrollbar-track {
  background-color: #e2e2e2;
  border-radius: 2px;
}
.default-theme ::-webkit-scrollbar-thumb {
  border-radius: 2px;
  background-color: #0000004d;
}
.default-theme ::-webkit-scrollbar-button:vertical {
  display: none;
}
.default-theme ::-webkit-scrollbar-thumb:vertical:hover {
  background-color: #00000059;
}
.default-theme ::-webkit-scrollbar-thumb:vertical:active {
  background-color: #00000061;
}
.default-theme h1,
.default-theme h2,
.default-theme h3,
.default-theme h4,
.default-theme h5,
.default-theme h6 {
  position: relative;
  word-break: break-all;
}
.default-theme h1 a,
.default-theme h2 a,
.default-theme h3 a,
.default-theme h4 a,
.default-theme h5 a,
.default-theme h6 a,
.default-theme h1 a:hover,
.default-theme h2 a:hover,
.default-theme h3 a:hover,
.default-theme h4 a:hover,
.default-theme h5 a:hover,
.default-theme h6 a:hover {
  color: inherit;
}
.default-theme ol > li {
  list-style: decimal;
}
.default-theme ul > li {
  list-style: disc;
}
.default-theme ol .li-task,
.default-theme ul .li-task {
  list-style-type: none;
}
.default-theme ol .li-task input,
.default-theme ul .li-task input {
  margin-left: -1.5em;
  margin-right: 0.1em;
}
.default-theme a {
  text-decoration: none;
}
.default-theme hr {
  height: 1px;
  margin: 10px 0;
  border: none;
  border-top: 1px solid #eaecef;
}
.default-theme div[inline] > .figure {
  padding-right: 0.5em;
}
.default-theme div[inline] > .figure img {
  padding: 0;
  border: none;
}
.default-theme .figure {
  margin: 0 0 1em;
  display: inline-flex;
  flex-direction: column;
  text-align: center;
}
.default-theme .figure .figcaption {
  color: #888;
  font-size: 0.875em;
  margin-top: 5px;
}
.default-theme h1,
.default-theme h2,
.default-theme h3,
.default-theme h4,
.default-theme h5,
.default-theme h6 {
  margin: 1.4em 0 0.8em;
  font-weight: 700;
}
.default-theme a {
  color: #2d8cf0;
  transition: color 0.3s;
}
.default-theme a:hover {
  color: #73d13d;
}
.default-theme img {
  margin: 0 auto;
  max-width: 100%;
}
.default-theme p {
  line-height: 1.6;
  margin: 0;
  padding: 0.5rem 0;
}
.default-theme p:empty {
  display: none;
}
.default-theme blockquote {
  margin: 20px 0;
  padding: 0.5em 1.2em;
  line-height: 2em;
  background-color: #ececec;
  border-left: 5px solid #35b378;
  display: block;
}
.default-theme blockquote p {
  padding: 0;
}
.default-theme table {
  overflow: auto;
  border-spacing: 0;
  border-collapse: collapse;
  margin-bottom: 1em;
}
.default-theme table tr th,
.default-theme table tr td {
  word-wrap: break-word;
  padding: 8px 14px;
  border: 1px solid #e6e6e6;
}
.default-theme table tr:nth-child(2n) {
  background-color: #fafafa;
}
.default-theme table tr:hover {
  background-color: #eee;
}
.default-theme ol,
.default-theme ul {
  margin: 0.6em 0;
  padding-left: 1.6em;
}
.default-theme ol li,
.default-theme ul li {
  line-height: 1.6;
  margin: 0.5em 0;
}
</style>
