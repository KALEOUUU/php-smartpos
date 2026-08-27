<script setup lang="ts">
import AppLayout from '@/layouts/AppLayout.vue';
import { Head, Link, useForm } from '@inertiajs/vue3';
import { Button } from '@/components/ui/button';

defineProps<{
    products: any[];
}>();

const breadcrumbs: any[] = [];

const form = useForm({});

const deleteProduct = (id: number) => {
    if (confirm('Are you sure you want to delete this product?')) {
        form.delete(`/products/${id}`);
    }
};
</script>

<template>
    <Head title="Products" />

    <AppLayout :breadcrumbs="breadcrumbs">
        <div class="p-6">
            <div class="flex justify-between items-center mb-6">
                <h1 class="text-2xl font-semibold">Products</h1>
                <Link href="/products/create">
                    <Button>Add New Product</Button>
                </Link>
            </div>

            <div class="bg-background border rounded-md overflow-hidden">
                <table class="w-full text-sm text-left">
                    <thead class="bg-muted text-muted-foreground border-b uppercase text-xs">
                        <tr>
                            <th class="px-6 py-3 font-medium">Name</th>
                            <th class="px-6 py-3 font-medium">Category</th>
                            <th class="px-6 py-3 font-medium">Price</th>
                            <th class="px-6 py-3 font-medium">Stock</th>
                            <th class="px-6 py-3 font-medium text-right">Actions</th>
                        </tr>
                    </thead>
                    <tbody class="divide-y">
                        <tr v-for="product in products" :key="product.id" class="hover:bg-muted/50 transition-colors">
                            <td class="px-6 py-4 font-medium">{{ product.name }}</td>
                            <td class="px-6 py-4 capitalize">{{ product.category }}</td>
                            <td class="px-6 py-4">Rp {{ product.price.toLocaleString('id-ID') }}</td>
                            <td class="px-6 py-4">{{ product.stock }}</td>
                            <td class="px-6 py-4 text-right space-x-2">
                                <Link :href="`/products/${product.id}/edit`">
                                    <Button variant="outline" size="sm">Edit</Button>
                                </Link>
                                <Button variant="destructive" size="sm" @click="deleteProduct(product.id)">Delete</Button>
                            </td>
                        </tr>
                        <tr v-if="products.length === 0">
                            <td colspan="5" class="px-6 py-8 text-center text-muted-foreground">
                                No products found.
                            </td>
                        </tr>
                    </tbody>
                </table>
            </div>
        </div>
    </AppLayout>
</template>
