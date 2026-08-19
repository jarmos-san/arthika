<script setup lang="ts">
  import { DialogOverlay, DialogPortal, DialogRoot } from "reka-ui";

  import type { AssetClass } from "~/openapi";

  interface Props {
    /** @description The asset class being edited. */
    asset: AssetClass;

    /** @description Whether the dialog is open. */
    open: boolean;
  }

  interface Emits {
    /** @description Fired when the dialog's open state changes. */
    "update:open": [open: boolean];
  }

  const props = defineProps<Props>();
  const emit = defineEmits<Emits>();

  /**
   * @description Forwards reka-ui's open-state changes to the parent.
   *
   * @param {boolean} open Whether the dialog is still open.
   */
  const onOpenChange = (open: boolean): void => {
    emit("update:open", open);
  };

  /**
   * @description Submit button handler: submits the updated asset information and closes the
   * form modal.
   */
  const onSubmit = (): void => {
    console.log(`Edited ${props.asset.name}`);
    emit("update:open", false);
  };

  /** @description Cancel button handler: closes the dialog. */
  const onClose = (): void => {
    emit("update:open", false);
  };
</script>

<template>
  <DialogRoot :open="props.open" @update:open="onOpenChange">
    <DialogPortal>
      <!-- The transparent overlay -->
      <DialogOverlay class="fixed inset-0 z-50 bg-stone-900/40" />

      <!-- The actual content of the modal -->
      <DialogContent
        class="fixed top-1/2 left-1/2 z-50 w-full max-w-md -translate-x-1/2 -translate-y-1/2 rounded-xl bg-white p-6 shadow-xl"
      >
        <!-- The modal title -->
        <DialogTitle
          class="text-lg font-semibold tracking-tight text-stone-800"
        >
          Edit {{ props.asset.name }}
        </DialogTitle>

        <!-- The modal description -->
        <DialogDescription class="mt-1.5 text-sm text-stone-500">
          Update the details for {{ props.asset.name }} below. Changes are saved
          when you submit the form.
        </DialogDescription>

        <!-- The button groups -->
        <div class="mt-6 flex justify-end gap-3">
          <!-- The cancel button -->
          <button class="btn-secondary" type="button" @click="onClose">
            Cancel
          </button>

          <!-- The submit button -->
          <button
            class="btn-primary"
            type="submit"
            @submit.prevent="onSubmit"
            @click="onSubmit"
          >
            Submit
          </button>
        </div>
      </DialogContent>
    </DialogPortal>
  </DialogRoot>
</template>
