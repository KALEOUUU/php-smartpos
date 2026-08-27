<script setup lang="ts">
import AppLayout from '@/layouts/AppLayout.vue';
import { Head, Link, useForm } from '@inertiajs/vue3';
import { Button } from '@/components/ui/button';
import { Input } from '@/components/ui/input';
import { Label } from '@/components/ui/label';

const props = defineProps<{
    product: {
        id: number;
        name: string;
        category: string;
        price: number;
        stock: number;
        description: string | null;
    };
}>();

const breadcrumbs: any[] = [];

const form = useForm({
    name: props.product.name,
    category: props.product.category,
    price: props.product.price,
    stock: props.product.stock,
    description: props.product.description || '',
});

const submit = () => {
    form.put(`/products/${props.product.id}`);
};
</script>

<template>
    <Head title="Edit Product" />

    <AppLayout :breadcrumbs="breadcrumbs">
        <div class="p-6 max-w-2xl mx-auto">
            <div class="mb-6">
                <h1 class="text-2xl font-semibold">Edit Product</h1>
            </div>

            <form @submit.prevent="submit" class="space-y-6">
                <div class="space-y-2">
                    <Label for="name">Product Name</Label>
                    <Input id="name" v-model="form.name" type="text" required />
                    <p class="text-sm text-red-500" v-if="form.errors.name">{{ form.errors.name }}</p>
                </div>

                <div class="space-y-2">
                    <Label for="category">Category</Label>
                    <select id="category" v-model="form.category" class="flex h-9 w-full rounded-md border border-input bg-transparent px-3 py-1 text-base shadow-sm transition-colors file:border-0 file:bg-transparent file:text-sm file:font-medium file:text-foreground placeholder:text-muted-foreground focus-visible:outline-none focus-visible:ring-1 focus-visible:ring-ring disabled:cursor-not-allowed disabled:opacity-50 md:text-sm" required>
                        <option value="makanan">Makanan</option>
                        <option value="minuman">Minuman</option>
                        <option value="snack">Snack</option>
                    </select>
                    <p class="text-sm text-red-500" v-if="form.errors.category">{{ form.errors.category }}</p>
                </div>

                <div class="grid grid-cols-2 gap-4">
                    <div class="space-y-2">
                        <Label for="price">Price (Rp)</Label>
                        <Input id="price" v-model="form.price" type="number" min="0" required />
                        <p class="text-sm text-red-500" v-if="form.errors.price">{{ form.errors.price }}</p>
                    </div>

                    <div class="space-y-2">
                        <Label for="stock">Stock</Label>
                        <Input id="stock" v-model="form.stock" type="number" min="0" required />
                        <p class="text-sm text-red-500" v-if="form.errors.stock">{{ form.errors.stock }}</p>
                    </div>
                </div>

                <div class="space-y-2">
                    <Label for="description">Description (Optional)</Label>
                    <textarea id="description" v-model="form.description" class="flex min-h-[80px] w-full rounded-md border border-input bg-background px-3 py-2 text-base ring-offset-background placeholder:text-muted-foreground focus-visible:outline-none focus-visible:ring-2 focus-visible:ring-ring focus-visible:ring-offset-2 disabled:cursor-not-allowed disabled:opacity-50 md:text-sm"></textarea>
                    <p class="text-sm text-red-500" v-if="form.errors.description">{{ form.errors.description }}</p>
                </div>

                <div class="flex items-center gap-4">
                    <Button type="submit" :disabled="form.processing">Update Product</Button>
                    <Link href="/products">
                        <Button variant="outline" type="button">Cancel</Button>
                    </Link>
                </div>
            </form>
        </div>
    </AppLayout>
</template>
