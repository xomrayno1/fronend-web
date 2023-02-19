<script setup>
import ProductService from '../../../service/ProductService';
import { ref, onBeforeMount } from 'vue';

const products = ref([]);
const loading = ref(true);
const filter = ref('');
const productService = new ProductService();

onBeforeMount(() => {
    productService
        .getProducts()
        .then((data) => {
            products.value = data;
            console.log(data);
            loading.value = false;
        })
        .catch((err) => console.log(err));
});

const clearFilter = () => {
    filter.value = '';
};

const searchFilter = () => {
    const value = filter.value;
    products.value = products.value.filter((item) => item.name.includes(value));
};
</script>

<template>
    <div class="grid">
        <div class="col-12">
            <div class="card">
                <h5 class="mb-5">Danh sách nhà cung cấp</h5>
                <div class="flex justify-content-between flex-column sm:flex-row">
                    <Button type="button" icon="pi pi-filter-slash" label="Clear" class="p-button-outlined mb-2" @click="clearFilter" />
                    <span class="p-input-icon-left mb-2">
                        <i class="pi pi-search" @click="searchFilter" />
                        <InputText v-model="filter" placeholder="Nhập tên hoặc mã" style="width: 100%" />
                    </span>
                </div>
                <DataTable :value="products" class="p-datatable-gridlines" :rows="10" :paginator="true" responsiveLayout="scroll" :loading="loading">
                    <template #header>
                        <Toolbar style="padding: 0px; border: 0px">
                            <template v-slot:start>
                                <Button label="Thêm" icon="pi pi-plus" class="mr-2" />
                                <i class="pi pi-bars p-toolbar-separator mr-2"></i>
                                <Button icon="pi pi-trash" class="p-button-warning mr-2" />
                                <Button icon="pi pi-print" class="p-button-danger" />
                            </template>
                        </Toolbar>
                    </template>
                    <template #empty> No customers found. </template>
                    <template #loading> Loading customers data. Please wait. </template>
                    <Column field="name" header="Tên sản phẩm" style="min-width: 12rem">
                        <template #body="{ data }">
                            {{ data.name }}
                        </template>
                    </Column>
                    <Column field="code" header="Mã sản phẩm" style="min-width: 12rem">
                        <template #body="{ data }">
                            {{ data.code }}
                        </template>
                    </Column>
                    <Column field="description" header="Mô tả sản phẩm" style="min-width: 12rem">
                        <template #body="{ data }">
                            {{ data.description }}
                        </template>
                    </Column>
                    <Column field="action" header=" + " style="min-width: 12rem">
                        <template>
                            <div></div>
                        </template>
                    </Column>
                </DataTable>
            </div>
        </div>
    </div>
</template>

<style scoped lang="scss">
@import '@/assets/demo/styles/badges.scss';

::v-deep(.p-datatable-frozen-tbody) {
    font-weight: bold;
}

::v-deep(.p-datatable-scrollable .p-frozen-column) {
    font-weight: bold;
}
</style>
