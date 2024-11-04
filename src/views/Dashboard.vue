<script setup>
import { useLayout } from '@/layout/composables/layout';
import { ProductService } from '@/service/ProductService';
import { onMounted, ref, watch } from 'vue';
import { NodeService } from '@/service/NodeService';

const { getPrimary, getSurface, isDarkTheme } = useLayout();

const products = ref(null);
const chartData = ref(null);
const chartOptions = ref(null);

const treeValue = ref(null);
const treeTableValue = ref(null);
const selectedTreeTableValue = ref(null);

onMounted(() => {
    NodeService.getTreeNodes().then((data) => (treeValue.value = data));
    NodeService.getTreeTableNodes().then((data) => (treeTableValue.value = data));
});
// const items = ref([
//     { label: 'Add New', icon: 'pi pi-fw pi-plus' },
//     { label: 'Remove', icon: 'pi pi-fw pi-trash' }
// ]);

onMounted(() => {
    ProductService.getProductsSmall().then((data) => (products.value = data));
    chartData.value = setChartData();
    chartOptions.value = setChartOptions();
});

function setChartData() {
    const documentStyle = getComputedStyle(document.documentElement);

    return {
        labels: ['Q1', 'Q2', 'Q3', 'Q4'],
        datasets: [
            {
                type: 'bar',
                label: 'Subscriptions',
                backgroundColor: documentStyle.getPropertyValue('--p-primary-400'),
                data: [4000, 10000, 15000, 4000],
                barThickness: 32
            },
            {
                type: 'bar',
                label: 'Advertising',
                backgroundColor: documentStyle.getPropertyValue('--p-primary-300'),
                data: [2100, 8400, 2400, 7500],
                barThickness: 32
            },
            {
                type: 'bar',
                label: 'Affiliate',
                backgroundColor: documentStyle.getPropertyValue('--p-primary-200'),
                data: [4100, 5200, 3400, 7400],
                borderRadius: {
                    topLeft: 8,
                    topRight: 8
                },
                borderSkipped: true,
                barThickness: 32
            }
        ]
    };
}

function setChartOptions() {
    const documentStyle = getComputedStyle(document.documentElement);
    const borderColor = documentStyle.getPropertyValue('--surface-border');
    const textMutedColor = documentStyle.getPropertyValue('--text-color-secondary');

    return {
        maintainAspectRatio: false,
        aspectRatio: 0.8,
        scales: {
            x: {
                stacked: true,
                ticks: {
                    color: textMutedColor
                },
                grid: {
                    color: 'transparent',
                    borderColor: 'transparent'
                }
            },
            y: {
                stacked: true,
                ticks: {
                    color: textMutedColor
                },
                grid: {
                    color: borderColor,
                    borderColor: 'transparent',
                    drawTicks: false
                }
            }
        }
    };
}

// const formatCurrency = (value) => {
//     return value.toLocaleString('en-US', { style: 'currency', currency: 'USD' });
// };

watch([getPrimary, getSurface, isDarkTheme], () => {
    chartData.value = setChartData();
    chartOptions.value = setChartOptions();
});
</script>

<template>
    <div class="card">
        <div class="flex" style="gap: 0.5rem; align-items: stretch">
            <i class="pi pi-book" style="font-size: 2.3rem"></i>
            <h2 class="font-semibold text-4xl mb-6">Библиотека</h2>
        </div>

        <div class="card flex flex-col gap-4 w-full" style="padding: initial">
            <Toolbar>
                <template #start>
                    <IconField>
                        <InputIcon>
                            <i class="pi pi-search" />
                        </InputIcon>
                        <InputText placeholder="Search" style="width: 60vh" />
                    </IconField>
                </template>

                <template #end>
                    <Button icon="pi pi-plus" class="mr-2" severity="secondary" text />
                    <Button icon="pi pi-print" class="mr-2" severity="secondary" text />
                    <Button icon="pi pi-upload" severity="secondary" text />
                    <Button icon="pi pi-upload" severity="secondary" text />
                    <Button icon="pi pi-upload" severity="secondary" text />
                </template>
            </Toolbar>
            <div class="font-semibold text-xl" style="border-bottom: 1px solid var(--surface-border)">Разделы</div>
            <div class="flex flex-col md:flex-row gap-4">
                <InputGroup>
                    <div class="layout-menu ul a">
                        <span>Раздел №1</span>
                        <i class="pi pi-fw pi-angle-right layout-submenu-toggler"></i>
                    </div>
                </InputGroup>
                <InputGroup>
                    <span>Раздел №1</span>
                </InputGroup>
            </div>
            <div class="flex flex-col md:flex-row gap-4">
                <InputGroup>
                    <span>Раздел №1</span>
                </InputGroup>
                <InputGroup>
                    <span>Раздел №1</span>
                </InputGroup>
            </div>
        </div>

        <div class="font-semibold text-xl mb-4" style="border-bottom: 1px solid var(--surface-border)">Несортированные</div>
        <TreeTable :value="treeTableValue" selectionMode="button" v-model:selectionKeys="selectedTreeTableValue">
            <Column field="name" header="Имя" :expander="true"></Column>
            <Column field="size" header="Дата"></Column>
            <Column field="type" header="Время"></Column>
        </TreeTable>
    </div>
</template>
