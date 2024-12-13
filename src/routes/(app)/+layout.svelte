<script lang="ts">
    import { page } from "$app/stores";
    import { House, Calendar, Settings, Pill } from "lucide-svelte";

    let { children } = $props();
</script>

<div class="flex flex-col h-screen">
    <!-- Main content -->
    <main class="flex-1 overflow-auto">
        {@render children()}
    </main>
    <!-- Bottom navigation -->
    <nav class="border-t bg-background">
        <div class="flex items-center justify-around h-14">
            <!-- Home -->
            <a
                href="/home"
                class="flex flex-col items-center gap-1 text-muted-foreground hover:text-foreground"
                class:active={$page.url.pathname === "/"}
            >
                <House />
                <span class="text-xs">Home</span>
            </a>
            <!-- Medications -->
            <a
                href="/medications"
                class="flex flex-col items-center gap-1 text-muted-foreground hover:text-foreground"
                class:active={$page.url.pathname === "/medications"}
            >
                <Pill />
                <span class="text-xs">Medications</span>
            </a>
            <!-- History -->
            <a
                href="/history"
                class="flex flex-col items-center gap-1 text-muted-foreground hover:text-foreground"
                class:active={$page.url.pathname === "/history"}
            >
                <Calendar />
                <span class="text-xs">History</span>
            </a>
            <!-- Settings -->
            <a
                href="/settings"
                class="flex flex-col items-center gap-1 text-muted-foreground hover:text-foreground"
                class:active={$page.url.pathname === "/settings"}
            >
                <Settings />
                <span class="text-xs">Settings</span>
            </a>
        </div>
    </nav>
</div>

<style>
    .active {
        --tw-text-opacity: 1;
        color: hsl(var(--foreground) / var(--tw-text-opacity));
    }
</style>
