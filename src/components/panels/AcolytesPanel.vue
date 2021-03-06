<template>
  <HubPanelWrap :title="headertext" :class="{ 'no-content': acolytes.length === 0 }">
    <b-list-group>
      <b-list-group-item
        :style="styleObject"
        v-for="(acolyte, index) in acolytes"
        :key="acolyte.id"
        v-bind:class="{
          'list-group-item-borderless': index !== acolytes.length - 1,
          'list-group-item-borderbottom': index === acolytes.length - 1,
        }"
      >
        <span :class="`badge badge-${healthLabel(acolyte)} pull-right`">
          <span :id="`${acolyte.id}-health`">{{ health(acolyte) }}</span
          >%
          <i v-b-tooltip class="fas fa-heartbeat" :title="healthStr" style="margin-left: 5px"></i>
        </span>
        <span class="pull-left">
          <b>
            <i
              class="far fa-eye faIcon"
              v-b-tooltip
              :title="`${acolyte.agentType} ${this.$t('acolytes.discovered')}`"
              :style="acolyteIcons"
              v-if="acolyte.isDiscovered"
            />
            <i
              class="far fa-eye-slash faIcon"
              v-b-tooltip
              :title="`${acolyte.agentType} ${discovery(acolyte)}`"
              :style="acolyteIcons"
              v-else
            />
            {{ acolyte.agentType }}
          </b>
        </span>
        <br />
        <div style="margin-top:2px">
          <span class="pull-left">
            <b>
              <span :id="`${acolyte.id}-loc`">
                {{ acolyte.isDiscovered ? '' : 'Last ' }} @{{ acolyte.lastDiscoveredAt || none }}
              </span>
            </b>
            | <b>Level: </b>{{ acolyte.rank }}
          </span>
          <b-badge :id="`${acolyte.id}-lastDicoveredTime`" variant="info" class="pull-right timebadage">
            {{ dayjs(acolyte.lastDiscoveredTime).fromNow() }}
          </b-badge>
        </div>
      </b-list-group-item>
      <NoDataItem v-if="acolytes.length === 0" :text="headertext" />
    </b-list-group>
  </HubPanelWrap>
</template>

<script>
import NoDataItem from '@/components/NoDataItem.vue';
import HubPanelWrap from '@/components/HubPanelWrap';
import dayjs from 'dayjs';

export default {
  name: 'AcolytesPanel',
  props: ['acolytes'],
  computed: {
    headertext() {
      return this.$t('acolytes.header');
    },
    none() {
      return this.$t('acolytes.na');
    },
    healthStr() {
      return this.$t('acolytes.health');
    },
  },
  methods: {
    dayjs: dayjs,
    discovery(acolyte) {
      return this.$t(`acolytes.${acolyte.isDiscovered ? 'discovered' : 'hiding'}`);
    },
    health(acolyte) {
      return (acolyte.healthPercent * 100).toFixed(2);
    },
    healthLabel(acolyte) {
      const health = this.health(acolyte);
      let labelClass = 'success';
      if (health <= 80 && health > 50) {
        labelClass = 'info';
      } else if (health <= 50 && health > 20) {
        labelClass = 'warning';
      } else if (health <= 20) {
        labelClass = 'danger';
      }
      return labelClass;
    },
  },
  data() {
    return {
      styleObject: {
        display: 'inline',
      },
      acolyteIcons: {
        'margin-right': '10px',
      },
    };
  },
  components: {
    NoDataItem,
    HubPanelWrap,
  },
};
</script>
