<script lang="ts">
    import { Skeleton } from "$lib/components/ui/skeleton";
</script>

<div class="flex flex-col h-full">
    <header
        class="bg-primary text-primary-foreground px-4 py-2 flex items-center justify-between"
    >
        <div>
            <span class="font-medium text-lg">History</span>
        </div>
    </header>
    <div class="flex-1 grid gap-6 p-4">
        <div class="flex flex-col justify-center items-center space-y-4">
            <Skeleton class="h-12 w-12 rounded-full" />
            <div class="space-y-2">
                <Skeleton class="h-4 w-[250px]" />
                <Skeleton class="h-4 w-[200px]" />
            </div>
        </div>
    </div>
</div>
