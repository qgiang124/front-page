<script setup lang="ts">
import { computed, defineProps, ref } from 'vue';
import ShoppingCart from './ShoppingCart.vue';
import {
  NGrid,
  NGridItem,
  NInput,
  NPageHeader,
  NButton,
  NModal,
  NCard,
  NDropdown
} from 'naive-ui';
const openModal = ref(false);
const openShoppingCart = ref(false);
const openCheckoutModal = ref(false);
const openEditModal = ref(false);
interface CartItem {
  id: number;
  name: string;
  price: number;
  quantity: number;
}

interface ProductItem {
  id: number;
  name: string;
  price: number;
  imageUrl: string;
  category: string;
}

const itemsInCart = ref<CartItem[]>([
  { id: 1, name: 'rose', price: 12, quantity: 2 },
  { id: 2, name: 'tulip', price: 15, quantity: 3 }
]);

const categories = ref([
  { label: 'A', key: 'A' },
  { label: 'B', key: 'B' },
  { label: 'C', key: 'C' },
  { label: 'Clear filter', key: '' }
]);

const filterKey = ref('');
const items = ref<ProductItem[]>([
  {
    id: 1,
    name: 'rose',
    price: 12,
    imageUrl:
      'https://images.unsplash.com/photo-1532713031318-db2d14e4b3e1?w=500&auto=format&fit=crop&q=60&ixlib=rb-4.0.3&ixid=M3wxMjA3fDB8MHxzZWFyY2h8Nnx8cm9zZSUyMGZsb3JhbCUyMHNob3B8ZW58MHx8MHx8fDA=',
    category: 'A'
  },
  {
    id: 2,
    name: 'tulip',
    price: 15,
    imageUrl:
      'https://images.unsplash.com/photo-1522431745718-cd31223a468d?w=500&auto=format&fit=crop&q=60&ixlib=rb-4.0.3&ixid=M3wxMjA3fDB8MHxzZWFyY2h8Mnx8dHVsaXAlMjBmbG9yYWwlMjBzaG9wfGVufDB8fDB8fHww',
    category: 'B'
  },
  {
    id: 3,
    name: 'sunflower',
    price: 10,
    imageUrl:
      'https://images.unsplash.com/photo-1542801204-141ec23989d7?w=500&auto=format&fit=crop&q=60&ixlib=rb-4.0.3&ixid=M3wxMjA3fDB8MHxzZWFyY2h8MTd8fHN1bmZsb3dlciUyMGJvcXVldHxlbnwwfHwwfHx8MA==',
    category: 'A'
  }
]);

const handleDecreaseQuant = (itemId: number) => {
  const item = itemsInCart.value.find((item) => item.id === itemId);
  if (item && item.quantity > 0) {
    item.quantity--;
  }
};

const handleIncreaseQuant = (itemId: number) => {
  const item = itemsInCart.value.find((item) => item.id === itemId);
  if (item) {
    item.quantity++;
  }
};
const newItem = ref({
  name: '',
  price: 0,
  imageUrl: 'no image',
  category: 'unknown'
});

function addItem() {
  console.log('Adding item');
  items.value.push({
    id: items.value.length + 1,
    name: newItem.value.name,
    price: newItem.value.price,
    imageUrl: newItem.value.imageUrl,
    category: newItem.value.category
  });
}

const updatePrice = () => {
  const itemToUpdate = items.value.find(
    (item: ProductItem) => item.id === selectedItem.value.id
  );
  if (itemToUpdate) {
    itemToUpdate.price = selectedItem.value.price;
  }
};

function deleteItem(itemId: number) {
  console.log('Deleting item');
  items.value = items.value.filter((item) => item.id !== itemId);
}

function cancelAddItem() {
  openModal.value = false;
}

const handleDeleteItemFromCart = (itemId: number) => {
  itemsInCart.value = itemsInCart.value.filter((item) => item.id !== itemId);
};

const handleAddToCart = (item: ProductItem) => {
  const itemToAdd = itemsInCart.value.find(
    (itemProduct) => itemProduct.name === item.name
  );
  if (itemToAdd) {
    itemToAdd.quantity += 1;
  } else {
    itemsInCart.value.push({
      id: items.value.length + 1,
      name: item.name,
      price: item.price,
      quantity: 1
    });
  }
  alert('Added to the shopping cart!');
};

const selectedItem = ref<ProductItem>({
  name: '',
  price: 0,
  imageUrl: '',
  category: '',
  id: -1
});

const filteredItems = computed(() => {
  if (filterKey.value == '') return items.value;
  else return items.value.filter((item) => item.category === filterKey.value);
});

const onSelect = function (key: string) {
  filterKey.value = key;
};
</script>
<template>
  <div class="floral-shop">
    <div class="header">
      <NPageHeader title="Floral Shop"></NPageHeader>
      <UserInfo></UserInfo>
      <div>
        <NModal v-model:show="openShoppingCart">
          <NCard
            style="width: 600px"
            title="Shopping Cart"
            :bordered="false"
            size="huge"
            role="dialog"
            aria-modal="true"
          >
            <ShoppingCart
              v-for="item in itemsInCart"
              :key="item.id"
              :cartItem="item"
              :onDelete="handleDeleteItemFromCart"
              :onNegative="handleDecreaseQuant"
              :onPositive="handleIncreaseQuant"
            />
            <div>
              <NButton type="primary" @click="openCheckoutModal = true">
                Check out
              </NButton>
              <NModal v-model:show="openCheckoutModal">
                <NCard
                  style="width: 600px"
                  title="Success!"
                  :bordered="false"
                  size="huge"
                  role="dialog"
                  aria-modal="true"
                >
                  <h1>Thank you for your purchase!</h1>
                </NCard>
              </NModal>
            </div>
          </NCard>
        </NModal>
      </div>
    </div>

    <div class="items-container">
      <div>
        <NDropdown trigger="hover" :options="categories" @select="onSelect">
          <NButton strong secondary type="primary">Filter by Category</NButton>
        </NDropdown>
        <NButton @click="openShoppingCart = true" type="info">
          Shopping Cart
        </NButton>
      </div>
      <div class="collection">
        <NGrid cols="4" x-gap="30px">
          <NGridItem v-for="item in filteredItems" :key="item.id" class="item">
            <img :src="item.imageUrl" class="item-image" />
            <div div class="item-title">
              <div>{{ item.name }}</div>
              <div>
                ${{ item.price }}
                <NButton
                  @click="
                    () => {
                      openEditModal = true;
                      selectedItem = { ...item };
                    }
                  "
                >
                  Edit
                </NButton>
              </div>
            </div>
            <div class="item-details">
              <div class="item-actions">
                <NButton
                  @click="deleteItem(item.id)"
                  strong
                  secondary
                  type="error"
                >
                  Delete
                </NButton>
                <NButton strong type="primary" @click="handleAddToCart(item)">
                  Add to Cart
                </NButton>
              </div>
            </div>
          </NGridItem>
        </NGrid>
      </div>
    </div>
    <NModal v-model:show="openEditModal">
      <NCard
        style="width: 600px"
        title="Edit price"
        :bordered="false"
        size="huge"
        role="dialog"
        aria-modal="true"
      >
        <div>
          Enter new price:
          <NInput type="text" v-model:value="selectedItem.price"></NInput>
          <NButton
            type="primary"
            @click="
              () => {
                openEditModal = false;
                updatePrice();
              }
            "
          >
            Save
          </NButton>
        </div>
      </NCard>
    </NModal>
    <div>
      <NButton @click="openModal = true">New Item</NButton>
      <NModal v-model:show="openModal">
        <NCard
          style="width: 600px"
          title="Add New Item"
          :bordered="false"
          size="huge"
          role="dialog"
          aria-modal="true"
        >
          <div>
            <label for="itemName">Item Name:</label>
            <input v-model="newItem.name" type="text" required />
          </div>
          <div>
            <label for="itemPrice">Item Price:</label>
            <input v-model="newItem.price" type="number" required />
          </div>
          <div>
            <label for="itemCategory">Item Category:</label>
            <input v-model="newItem.category" type="text" required />
          </div>
          <NButton @click="addItem" type="success" class="add-item-btn">
            Add New Item
          </NButton>
          <NButton @click="cancelAddItem">Cancel</NButton>
        </NCard>
      </NModal>
    </div>
  </div>
</template>

<style scoped>
.floral-shop {
  display: flex;
  flex-wrap: wrap;
  flex-direction: column;
  justify-content: space-around;
  margin: 40px 60px;
}
.header {
  display: flex;
  justify-content: space-between;
}

.collection {
  display: flex;
  flex-direction: column;
}

.items-container {
  display: flex;
  margin-top: 20px;
  flex: 1;
  width: 100%;
  flex-direction: column;
}

.item {
  display: flex;
  flex-direction: column;
  align-items: center;
  margin-bottom: 10px;
  padding: 10px;
  border: 1px solid #ddd;
  border-radius: 8px;
  position: relative;
  align-items: center;
  justify-content: center;
}

.item-image {
  width: 100%;
  height: 200px;
  object-fit: cover;
  display: flex;
  justify-content: center;
}

.item-details {
  flex: 1;
  display: flex;
  justify-content: space-between;
  /* min-width: 48px; */
  margin: 8px 20px;
  width: 100%;
}

.item-actions {
  display: flex;
  flex-wrap: wrap;
  gap: 10px;
  flex: 1;
  width: 100%;
  justify-content: space-between;
}

.add-item-btn {
  margin-top: 20px;
  flex: 1;
}

.item-labels {
  margin-bottom: 16px;
}

.item.item-title {
  display: flex;
  /* gap: 50px; */
  justify-content: center;
  align-items: center;
}
</style>
