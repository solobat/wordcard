<template>
  <el-form style="margin: 20px 15px; min-height: 150px;" ref="config" :model="config">
    <el-row>
      <el-col :span="6">
        <el-form-item :label="i18n.general.engine">
          <el-select v-model="config.engine" :placeholder="i18n.base.choose">
            <el-option
              v-for="(item, index) in TRANSLATE_ENGINS"
              :key="index"
              :label="item.label"
              :value="item.value"
            ></el-option>
          </el-select>
        </el-form-item>
      </el-col>
      <el-col :span="6">
        <el-form-item :label="i18n.general.source">
          <el-select
            :disabled="config.autoSetFrom"
            v-model="config.from"
            filterable
            :placeholder="i18n.base.choose"
          >
            <el-option v-for="(item, index) in codeList" :key="index" :label="item" :value="item"></el-option>
          </el-select>
        </el-form-item>
      </el-col>
      <el-col :span="6">
        <el-form-item :label="i18n.general.target">
          <el-select v-model="config.to" filterable :placeholder="i18n.base.choose">
            <el-option v-for="(item, index) in codeList" :key="index" :label="item" :value="item"></el-option>
          </el-select>
          <el-tooltip effect="dark" :content="i18n.general.langTips" placement="top-start">
            <a
              class="code-help"
              href="https://cloud.google.com/translate/docs/languages"
              target="_blank"
            >?</a>
          </el-tooltip>
        </el-form-item>
      </el-col>
    </el-row>
    <el-row>
      <el-col :span="6">
        <el-form-item :label="i18n.general.autoSource">
          <el-switch v-model="config.autoSetFrom" on-color="#20a0ff"></el-switch>
        </el-form-item>
      </el-col>
      <el-col :span="6">
        <el-form-item :label="i18n.general.dblclickTrigger">
          <el-switch v-model="config.dblclick2trigger" on-color="#20a0ff"></el-switch>
        </el-form-item>
      </el-col>
      <el-col :span="6">
        <el-form-item :label="i18n.general.holdCtrl">
          <el-tooltip effect="dark" :content="i18n.general.holdCtrlTips" placement="top-start">
            <el-switch v-model="config.withCtrlOrCmd" on-color="#20a0ff"></el-switch>
          </el-tooltip>
        </el-form-item>
      </el-col>
    </el-row>
    <el-row>
      <el-col :span="6">
        <el-form-item :label="i18n.general.autocutSentence">
          <el-tooltip
            class="item"
            effect="dark"
            :content="i18n.general.autocutSentenceTips"
            placement="top-start"
          >
            <el-switch v-model="config.autocut" on-color="#20a0ff"></el-switch>
          </el-tooltip>
        </el-form-item>
      </el-col>
      <el-col :span="6">
        <el-form-item :label="i18n.general.cardFontSize">
          <el-select v-model="config.cardFontSize" :placeholder="i18n.base.choose">
            <el-option
              v-for="item in CARD_FONTSIZE_OPTIONS"
              :key="item.value"
              :label="item.label"
              :value="item.value"
            ></el-option>
          </el-select>
        </el-form-item>
      </el-col>
      <el-col :span="6">
        <el-form-item :label="i18n.general.precisionFirst">
          <el-tooltip
            class="item"
            effect="dark"
            :content="i18n.general.precisionFirstTips"
            placement="top-start"
          >
            <el-switch v-model="config.precisionFirst" on-color="#20a0ff"></el-switch>
          </el-tooltip>
        </el-form-item>
      </el-col>
    </el-row>
    <el-row>
      <el-col :span="6">
        <el-form-item :label="i18n.general.ominboxEnterShowSentence">
          <el-switch v-model="config.alertOnOmniboxInputEntered" on-color="#20a0ff"></el-switch>
        </el-form-item>
      </el-col>
      <el-col :span="6">
        <el-form-item :label="i18n.general.autoSync">
          <el-switch v-model="config.autoSync" on-color="#20a0ff"></el-switch>
        </el-form-item>
      </el-col>
    </el-row>
    <el-form-item>
      <el-button type="primary" @click.native.prevent="handleConfigSubmit">{{ i18n.base.save }}</el-button>
    </el-form-item>
  </el-form>
</template>

<script>
import browser from "webextension-polyfill";
import { WORD_LEVEL, CARD_FONTSIZE_OPTIONS } from "@/js/constant/options";
import { TRANSLATE_ENGINS } from "@/js/constant/options";
import { codeList } from "@/js/constant/code";

export default {
  props: ['i18n', 'config'],

  data() {
    return {
      CARD_FONTSIZE_OPTIONS,
      TRANSLATE_ENGINS,
      codeList
    }
  },

  methods: {
    handleConfigSubmit() {
      this.saveConfig();
    },

    saveConfig: function(silent) {
      let self = this;
      let newConfig = JSON.parse(JSON.stringify(this.config));

      browser.storage.sync
        .set({
          config: newConfig
        })
        .then(resp => {
          if (!silent) {
            this.$message(this.i18n.msg.saveok);
          }
        });
    },
  }
};
</script>

<style>
</style>