<!--**
 * 2007-2020 PrestaShop and Contributors
 *
 * NOTICE OF LICENSE
 *
 * This source file is subject to the Academic Free License 3.0 (AFL-3.0)
 * that is bundled with this package in the file LICENSE.txt.
 * It is also available through the world-wide-web at this URL:
 * https://opensource.org/licenses/AFL-3.0
 * If you did not receive a copy of the license and are unable to
 * obtain it through the world-wide-web, please send an email
 * to license@prestashop.com so we can send you a copy immediately.
 *
 * @author    PrestaShop SA <contact@prestashop.com>
 * @copyright 2007-2020 PrestaShop SA and Contributors
 * @license   https://opensource.org/licenses/AFL-3.0 Academic Free License 3.0 (AFL-3.0)
 * International Registered Trademark & Property of PrestaShop SA
 *-->
<template>
  <div>
    <template v-if="!onboardingIsCompleted">
      <label class="text-muted">
        {{ $t('block.payment-status.disabled') }}
      </label>
    </template>
    <template v-else-if="cardIsInReview">
      <p v-if="displayLabels">
        {{ $t('block.payment-status.creditCardLabelPending') }}
      </p>
      <label
        v-else
        class="text-warning"
      >
        <i class="material-icons">error_outline</i> {{ $t('block.payment-status.approvalPending') }}
      </label>
    </template>
    <template v-else-if="cardStatus === 'SUBSCRIBED'">
      <p v-if="displayLabels">
        {{ $t('block.payment-status.creditCardLabelLive') }}
      </p>
      <label
        v-else
        class="text-success"
      >
        <i class="material-icons">check</i> {{ $t('block.payment-status.live') }}
      </label>
    </template>
    <template v-else-if="cardStatus === 'LIMITED'">
      <p v-if="displayLabels">
        {{ $t('block.payment-status.creditCardLabelLimited') }}
      </p>
      <label
        v-else
        class="text-warning"
      >
        <i class="material-icons">error_outline</i> {{ $t('block.payment-status.limited') }}
      </label>
    </template>
    <template v-else-if="cardStatus === 'DENIED'">
      <p v-if="displayLabels">
        {{ $t('block.payment-status.creditCardLabelDenied') }}
      </p>
      <label
        v-else
        class="text-danger"
      >
        <i class="material-icons">error_outline</i> {{ $t('block.payment-status.denied') }}
      </label>
    </template>
  </div>
</template>

<script>
  export default {
    name: 'CardStatus',
    props: {
      displayLabels: {
        type: Boolean,
        required: false,
        default: false,
      },
    },
    computed: {
      onboardingIsCompleted() {
        return this.$store.state.paypal.onboardingCompleted
          && this.$store.state.firebase.onboardingCompleted;
      },
      cardIsInReview() {
        const {cardIsActive} = this.$store.state.paypal;
        if (cardIsActive === 'IN_REVIEW'
          || cardIsActive === 'NEED_MORE_DATA'
          || !this.$store.state.paypal.emailIsValid) {
          return true;
        }
        return false;
      },
      cardStatus() {
        return this.$store.state.paypal.cardIsActive;
      },
    },
  };
</script>
