<template>
  <HubPanelWrap :title="headertext" class="conclave">
    <b-list-group v-if="conclave && conclave.length != 0">
      <b-list-group-item
        :style="styleObject"
        v-for="(challenge, index) in activeChallenges"
        :key="challenge.id"
        v-bind:class="{
          'list-group-item-borderless': index !== activeChallenges.length - 1,
          'pb-0': index !== activeChallenges.length - 1,
          'list-group-item-borderbottom': index === activeChallenges.length - 1,
        }"
      >
        <span class="pull-left">
          <HubImg
            :src="image(challenge)"
            :name="type(challenge)"
            class="li-mission-decorator li-mission-decorator-lg"
            :height="'24px'"
            width="24px"
          />
        </span>
        <span v-b-tooltip.right :title="challenge.asString" class="pull-left">
          {{ challenge.description }}
        </span>

        <TimeBadge class="pull-right" :starttime="challenge.activation" :endtime="challenge.expiry" :interval="1000" />
      </b-list-group-item>
    </b-list-group>
    <b-list-group v-if="!conclave || conclave.length === 0">
      <NoDataItem :text="headertext" />
    </b-list-group>
  </HubPanelWrap>
</template>

<script>
import TimeBadge from '@/components/TimeBadge.vue';
import NoDataItem from '@/components/NoDataItem.vue';
import HubPanelWrap from '@/components/HubPanelWrap';
import HubImg from '@/components/HubImg';

import conclaveimg from '@/assets/img/conclave/conclave.webp';
import lunaro from '@/assets/img/conclave/lunaro.webp';
import cephaloncapture from '@/assets/img/conclave/cephaloncapture.webp';
import annihilation from '@/assets/img/conclave/annihilation.webp';
import teamannihilation from '@/assets/img/conclave/teamannihilation.webp';

export default {
  name: 'ConclavePanel',
  props: ['conclave'],
  computed: {
    headertext() {
      return this.$t('conclave.header');
    },
    activeChallenges() {
      return this.conclave
        .filter((challenge) => !challenge.rootChallenge)
        .sort((a, b) => {
          return a.category == 'week' || b.category == 'week' ? 1 : a.mode.localeCompare(b.mode);
        });
    },
  },
  methods: {
    type: function(challenge) {
      if (challenge.mode == 'Team Annihilation') {
        return this.$t('conclave.teamannihilation');
      } else if (challenge.mode == 'Annihilation') {
        return this.$t('conclave.annihilation');
      } else if (challenge.mode == 'Capture the Cephalon') {
        return this.$t('conclave.cephaloncapture');
      } else if (challenge.mode == 'Lunaro') {
        return this.$t('conclave.lunaro');
      } else if (challenge.category == 'week') {
        return this.$t('conclave.weekly');
      } else {
        return this.$t('conclave.header');
      }
    },
    image: function(challenge) {
      if (challenge.mode == 'Team Annihilation') {
        return teamannihilation;
      } else if (challenge.mode == 'Annihilation') {
        return annihilation;
      } else if (challenge.mode == 'Capture the Cephalon') {
        return cephaloncapture;
      } else if (challenge.mode == 'Lunaro') {
        return lunaro;
      } else {
        return conclaveimg;
      }
    },
  },
  data() {
    return {
      styleObject: {
        display: 'inline',
      },
      conclaveimg: conclaveimg,
      lunaro: lunaro,
      annihilation: annihilation,
      cephaloncapture: cephaloncapture,
      teamannihilation: teamannihilation,
    };
  },
  components: {
    TimeBadge,
    NoDataItem,
    HubPanelWrap,
    HubImg,
  },
};
</script>
