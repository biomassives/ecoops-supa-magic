<script setup lang="ts">
import WelcomeItem from './WelcomeItem.vue'
import DocumentationIcon from './icons/IconDocumentation.vue'
import EcosystemIcon from './icons/IconEcosystem.vue'
import CommunityIcon from './icons/IconCommunity.vue'
import SupportIcon from './icons/IconSupport.vue'
</script>

<template>
  <WelcomeItem>
    <template #icon>
      <DocumentationIcon />
    </template>
    <template #heading>Checkins</template>

    Track activities that generate biodiversity credits & NFT tokens.
  </WelcomeItem>

  <WelcomeItem>
    <template #icon>
      <CommunityIcon />
    </template>
    <template #heading>Verification</template>

    Community verifiers are trained to be experts in techniques most appropriate for their regional
    context.
  </WelcomeItem>

  <WelcomeItem>
    <template #icon>
      <EcosystemIcon />
    </template>
    <template #heading>Techniques</template>

    Biodiversity credit-worthy activities empower those effected by pollution & climate to restore
    damaged environments.
  </WelcomeItem>

  <WelcomeItem>
    <template #icon>
      <SupportIcon />
    </template>
    <template #heading>Transparency</template>

    Participants provide checkin photos & recordings to show details about steps taken toward
    achieving project milestones.
  </WelcomeItem>
</template>

