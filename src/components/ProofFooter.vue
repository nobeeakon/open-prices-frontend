<template>
  <v-row>
    <v-col :cols="userIsProofOwner ? '11' : '12'">
      <ProofTypeChip class="mr-1" :proof="proof" />
      <LocationChip class="mr-1" :location="proof.location" :locationId="proof.location_id" :readonly="readonly" />
      <ProofDateChip v-if="proof.date" class="mr-1" :proof="proof" />
      <PriceCountChip :count="proof.price_count" :withLabel="true" @click="goToProof()" />
      <CurrencyChip v-if="proof.currency" class="mr-1" :proof="proof" />
      <RelativeDateTimeChip :dateTime="proof.created" />
    </v-col>
  </v-row>

  <ProofActionMenuButton v-if="!readonly && !hideProofActions && userIsProofOwner" :proof="proof" />
</template>

<script>
import { defineAsyncComponent } from 'vue'
import { mapStores } from 'pinia'
import { useAppStore } from '../store'

export default {
  components: {
    ProofTypeChip: defineAsyncComponent(() => import('../components/ProofTypeChip.vue')),
    LocationChip: defineAsyncComponent(() => import('../components/LocationChip.vue')),
    PriceCountChip: defineAsyncComponent(() => import('../components/PriceCountChip.vue')),
    ProofDateChip: defineAsyncComponent(() => import('../components/ProofDateChip.vue')),
    CurrencyChip: defineAsyncComponent(() => import('../components/CurrencyChip.vue')),
    RelativeDateTimeChip: defineAsyncComponent(() => import('../components/RelativeDateTimeChip.vue')),
    ProofActionMenuButton: defineAsyncComponent(() => import('../components/ProofActionMenuButton.vue'))
  },
  props: {
    proof: {
      type: Object,
      default: null
    },
    hideProofActions: {
      type: Boolean,
      default: false,
    },
    readonly: {
      type: Boolean,
      default: false,
    },
  },
  computed: {
    ...mapStores(useAppStore),
    username() {
      return this.appStore.user.username
    },
    userIsProofOwner() {
      return this.username && (this.proof.owner === this.username)
    },
  },
  methods: {
    goToProof() {
      if (this.readonly || !this.userIsProofOwner) {
        return
      }
      this.$router.push({ path: `/proofs/${this.proof.id}` })
    },
  }
}
</script>
