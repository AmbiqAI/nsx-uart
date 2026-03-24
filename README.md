# nsx-uart

`nsx-uart` provides an optional NSX wrapper for Ambiq UART usage across the
supported Apollo families.

Purpose:
- preserve a migration-friendly `ns_uart_*` API surface
- hide family-specific UART differences behind one module
- allow NSX apps to opt into the wrapper instead of using raw Ambiq HAL calls

This module depends on NSX shim layers that already normalize parts of the
legacy neuralSPOT behavior:
- `nsx-core`
- `nsx-harness`
- `nsx-utils`

It is intentionally optional and not part of the baseline starter profile.
