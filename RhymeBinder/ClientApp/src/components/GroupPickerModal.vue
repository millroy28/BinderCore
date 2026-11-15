<script setup>import { onMounted, onUnmounted } from 'vue'

const props = defineProps({
    groups: Array
})

const emit = defineEmits(['close'])

function selectGroup(savedViewId) {
    window.selected_action_form_submit('GroupFilter', savedViewId)
}

function closeOnEscape(event) {
    if (event.key === 'Escape') {
        emit('close')
    }
}

onMounted(() => {
    document.addEventListener('keydown', closeOnEscape)
})

onUnmounted(() => {
    document.removeEventListener('keydown', closeOnEscape)
})</script>

<template>
    <div class="group-picker-backdrop" @click.self="emit('close')">
        <div class="group-picker-modal"
             role="dialog"
             aria-modal="true"
             aria-labelledby="group-picker-title">
            <div class="group-picker-header">
                <div id="group-picker-title">Groups</div>

                <button type="button"
                        class="group-picker-close"
                        aria-label="Close"
                        @click="emit('close')">
                    Close
                </button>
            </div>

            <div class="group-picker-content">
                <a v-for="group in props.groups"
                   :key="group.savedViewId"
                   class="group-picker-item"
                   href="#"
                   @click.prevent="selectGroup(group.savedViewId)">
                    {{ group.groupTitle }}
                </a>
            </div>
        </div>
    </div>
</template>

<style scoped>
    .group-picker-backdrop {
        position: fixed;
        inset: 0;
        z-index: 100;
        display: flex;
        align-items: flex-start;
        justify-content: center;
        padding: 8vh 20px 20px;
        background: rgba(0, 0, 0, 0.25);
        pointer-events: all;
    }

    .group-picker-modal {
        width: min(520px, 100%);
        max-height: 80vh;
        display: flex;
        flex-direction: column;
        border: 2px solid var(--color-border-dark);
        background: var(--color-bg-body);
        box-shadow: 0 4px 8px rgba(0, 0, 0, 0.2), 0 6px 20px rgba(0, 0, 0, 0.19);
    }

    .group-picker-header {
        display: flex;
        align-items: center;
        justify-content: space-between;
        padding: 4px 12px;
        background: var(--color-bg-accent);
        font-size: larger;
        font-weight: 700;
    }

    .group-picker-close {
        border: 0;
        background: transparent;
        font-size: 1.4rem;
        line-height: 1;
        cursor: pointer;
    }

    .group-picker-content {
        display: grid;
        grid-template-columns: repeat(auto-fit, minmax(180px, 1fr));
        gap: 2px 12px;
        overflow-y: auto;
        padding: 12px;
        pointer-events: all;
    }

    .group-picker-item {
        display: block;
        overflow: hidden;
        padding: 6px 8px;
        color: inherit;
        text-decoration: none;
        text-overflow: ellipsis;
        white-space: nowrap;
    }

        .group-picker-item:hover,
        .group-picker-item:focus {
            background: var(--color-bg-hover);
            outline: none;
        }
</style>