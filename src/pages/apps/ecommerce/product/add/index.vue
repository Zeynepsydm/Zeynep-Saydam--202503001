<script setup>
import { ref } from 'vue'

const optionCounter = ref(1)
const dropZoneRef = ref()
const fileData = ref([])
const { onChange } = useFileDialog({ accept: 'image/*' })
function onDrop(DroppedFiles) {
  DroppedFiles?.forEach(file => {
    if (file.type.slice(0, 6) !== 'image/') {
      alert('Only image files are allowed')
      
      return
    }
    fileData.value.push({
      file,
      url: useObjectUrl(file).value ?? '',
    })
  })
}
onChange(selectedFiles => {
  if (!selectedFiles)
    return
  for (const file of selectedFiles) {
    fileData.value.push({
      file,
      url: useObjectUrl(file).value ?? '',
    })
  }
})
useDropZone(dropZoneRef, onDrop)

const content = ref(`<p>
    Keep your account secure with authentication step.
    </p>`)

const activeTab = ref('Restock')
const isTaxChargeToProduct = ref(true)

const shippingList = [
  {
    desc: 'You\'ll be responsible for product delivery.Any damage or delay during shipping may cost you a Damage fee',
    title: 'Fulfilled by Seller',
    value: 'Fulfilled by Seller',
  },
  {
    desc: 'Your product, Our responsibility.For a measly fee, we will handle the delivery process for you.',
    title: 'Fulfilled by Company name',
    value: 'Fulfilled by Company name',
  },
]

const shippingType = ref('Fulfilled by Company name')
const deliveryType = ref('Worldwide delivery')

const selectedAttrs = ref([
  'Biodegradable',
  'Expiry Date',
])

const inventoryTabsData = [
  {
    icon: 'tabler-cube',
    title: 'Restock',
    value: 'Restock',
  },
  {
    icon: 'tabler-car',
    title: 'Shipping',
    value: 'Shipping',
  },
  {
    icon: 'tabler-map-pin',
    title: 'Global Delivery',
    value: 'Global Delivery',
  },
  {
    icon: 'tabler-world',
    title: 'Attributes',
    value: 'Attributes',
  },
  {
    icon: 'tabler-lock',
    title: 'Advanced',
    value: 'Advanced',
  },
]
</script>

<template>
  <div>
    <div class="d-flex flex-wrap justify-start justify-sm-space-between gap-y-4 gap-x-6 mb-6">
      <div class="d-flex flex-column justify-center">
        <h4 class="text-h4 font-weight-medium">
          Yeni Ürün Ekle
        </h4>
        <div class="text-body-1">
         
        </div>
      </div>

      <div class="d-flex gap-4 align-center flex-wrap">
     
        <VBtn>Ürünü Gönder</VBtn>
      </div>
    </div>

    <VRow>
      <VCol md="8">
        <!-- 👉 Product Information -->
        <VCard
          class="mb-6"
          title="Ürün Açıklaması"
        >
          <VCardText>
            <VRow>
              <VCol cols="12">
                <AppTextField
                  label="Adı"
                  placeholder=""
                />
              </VCol>
              <VCol
                cols="12"
                md="6"
              >
                <AppTextField
                  label="SKU"
                  placeholder=""
                />
              </VCol>
              <VCol
                cols="12"
                md="6"
              >
                <AppTextField
                  label="Barkod"
                  placeholder=""
                />
              </VCol>
              <VCol>
                <span class="mb-1">Açıklama</span>
                <ProductDescriptionEditor
                  v-model="content"
                  placeholder=""
                  class="border rounded"
                />
              </VCol>
            </VRow>
          </VCardText>
        </VCard>

        <!-- 👉 Media -->
        <VCard class="mb-6">
          <VCardItem>
            <template #title>
              Ürün Resmi
            </template>
            <template #append>
              <span class="text-primary font-weight-medium text-sm cursor-pointer">Resim URL Ekle</span>
            </template>
          </VCardItem>

          <VCardText>
            <DropZone />
          </VCardText>
        </VCard>

        <!-- 👉 Variants -->
        <VCard
          title="Varyant"
          class="mb-6"
        >
          <VCardText>
            <template
              v-for="i in optionCounter"
              :key="i"
            >
              <VRow>
                <VCol
                  cols="12"
                  md="4"
                >
                  <AppSelect
                    :items="['Size', 'Color', 'Weight', 'Smell']"
                    placeholder="Varyant Seç"
                    label="Opsiyon"
                  />
                </VCol>
                <VCol
                  cols="12"
                  md="8"
                  class="d-flex align-self-end"
                >
                  <AppTextField
                    placeholder="38"
                    type="number"
                  />
                </VCol>
              </VRow>
            </template>

            <VBtn
              class="mt-6"
              prepend-icon="tabler-plus"
              @click="optionCounter++"
            >
             Başka Opsiyon Ekle
            </VBtn>
          </VCardText>
        </VCard>

        <!-- 👉 Inventory -->
      </VCol>

      <VCol
        md="4"
        cols="12"
      >
        <!-- 👉 Pricing -->
        <VCard
          title="Ücret"
          class="mb-6"
        >
          <VCardText>
            <AppTextField
              label="Fiyat"
              placeholder=""
              class="mb-6"
            />
            <AppTextField
              label="İndirimli Fiyat"
              placeholder=""
              class="mb-6"
            />

            <VCheckbox
              v-model="isTaxChargeToProduct"
              label="Vergi Dahil"
            />

            <VDivider class="my-2" />

            <div class="d-flex flex-raw align-center justify-space-between ">
              <span>Stok Varmı</span>
              <VSwitch density="compact" />
            </div>
          </VCardText>
        </VCard>

        <!-- 👉 Organize -->
        <VCard title="Organize">
          <VCardText>
            <div class="d-flex flex-column gap-y-4">
             
              <div>
                <VLabel class="d-flex">
                  <div class="d-flex text-sm justify-space-between w-100">
                    <div class="text-high-emphasis">
                      Kategori
                    </div>
                  </div>
                </VLabel>

                <div class="d-flex gap-x-4">
                  <AppSelect
                    placeholder="Kategori Seç"
                    :items="['Household', 'Office', 'Electronics', 'Management', 'Automotive']"
                  />
                  <VBtn
                    rounded
                    icon="tabler-plus"
                    variant="tonal"
                  />
                </div>
              </div>
              <AppSelect
                placeholder="Alt Kategori"
                label="Alt Kategori"
                :items="['Men\'s Clothing', 'Women\'s Clothing', 'Kid\'s Clothing']"
              />
              <AppSelect
                placeholder="Statü seç"
                label="Satatü"
                :items="['Published', 'Inactive', 'Scheduled']"
              />
              <AppTextField
                label="Tag"
                placeholder="Fashion, Trending, Summer"
              />
            </div>
          </VCardText>
        </VCard>
      </VCol>
    </VRow>
  </div>
</template>

<style lang="scss" scoped>
  .drop-zone {
    border: 2px dashed rgba(var(--v-theme-on-surface), 0.12);
    border-radius: 6px;
  }
</style>

<style lang="scss">
.inventory-card {
  .v-tabs.v-tabs-pill {
    .v-slide-group-item--active.v-tab--selected.text-primary {
      h6 {
        color: #fff !important;
      }
    }
  }

  .v-radio-group,
  .v-checkbox {
    .v-selection-control {
      align-items: start !important;
    }

    .v-label.custom-input {
      border: none !important;
    }
  }
}
</style>
