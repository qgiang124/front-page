<script setup lang="ts">
import { ref, defineProps } from 'vue';
import FrontPage from './FrontPage.vue';
import { NButton, NInputNumber, NListItem, NModal, NCard } from 'naive-ui';

interface CartItem {
  id: number;
  name: string;
  price: number;
  quantity: number;
}
const props = defineProps<{
  cartItem: CartItem;
  onDelete: (id: number) => void;
  onPositive: (id: number) => void;
  onNegative: (id: number) => void;
}>();

const handleDelete = function () {
  props.onDelete(props.cartItem.id);
};

function handleNegative() {
  props.onNegative(props.cartItem.id);
}

function handlePositive() {
  props.onPositive(props.cartItem.id);
}
</script>

<template>
  <div class="shopping-cart">
    <h2>{{ cartItem.name }}</h2>
    <div class="item-details">
      <label>${{ cartItem.price }}</label>
      <div class="action-cart">
        <div class="quantity-cart">
          <slot class="negative" size="small">
            <NButton @click="handleNegative">-</NButton>
          </slot>
          <label>{{ cartItem.quantity }}</label>
          <slot class="positive" size="small">
            <NButton @click="handlePositive">+</NButton>
          </slot>
        </div>
        <NButton strong secondary type="error" @click="handleDelete">
          Delete
        </NButton>
      </div>
    </div>
  </div>
</template>

<style>
.shopping-cart {
  margin: 8px;
}
.item-details {
  display: flex;
  justify-content: space-between;
  flex: 1;
}
.action-cart {
  display: flex;
  margin: 8px;
  gap: 16px;
}
.quantity-cart {
  display: flex;
  align-items: center;
  gap: 16px;
}
</style>
