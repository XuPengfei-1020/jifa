<!--
    Copyright (c) 2023 Contributors to the Eclipse Foundation

    See the NOTICE file(s) distributed with this work for additional
    information regarding copyright ownership.

    This program and the accompanying materials are made available under the
    terms of the Eclipse Public License 2.0 which is available at
    http://www.eclipse.org/legal/epl-2.0

    SPDX-License-Identifier: EPL-2.0
 -->
<script setup lang="ts">
import DownloadLink from '@/components/common/DownloadLink.vue';
import { ElMessageBox } from 'element-plus';
import { SetUp, Delete } from '@element-plus/icons-vue';
import { t } from '@/i18n/i18n';
import { useAnalysisApiRequester } from '@/composables/analysis-api-requester';
import { Phase, useAnalysisStore } from '@/stores/analysis';

const { request } = useAnalysisApiRequester();

const analysis = useAnalysisStore();

function showSetup() {
  analysis.setShowSetupPage(true)
}

function discard() {
  ElMessageBox.confirm(t('common.alertBeforeDiscard'))
    .then(() => {
      request('clean').then(() => {
        useAnalysisStore().leaveGuard = false;
        location.reload();
      })
    })
    .catch(() => {});
}

</script>
<template>
  <el-divider direction="vertical" />
  <DownloadLink />

  <template v-if="analysis.phase === Phase.SUCCESS || analysis.phase === Phase.FAILURE">
    <el-divider direction="vertical" />

    <el-button link class="ej-header-button" :icon="SetUp" @click="showSetup">
      {{ t('analysis.setting') }}
    </el-button>
  </template>
  <template v-if="analysis.phase === Phase.SUCCESS">
    <el-divider direction="vertical" />
    <el-button link class="ej-header-button" :icon="Delete" @click="discard">
      {{ t('analysis.discard') }}
    </el-button>
  </template>
</template>
