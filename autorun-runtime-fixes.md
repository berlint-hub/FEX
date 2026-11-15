=== Autorun runtime fixes (from user message) ===
Branch reference: arena/21bcd47b-autorun (commit aeea734)

1. Placeholder TLS pages (< 4 GB condition)
File: horizon-wine/source/runtime.c
Location: hold_thread_local_pages (~3379), condition (j == i) (~3404)
Change: add tls_page[i] < 0x100000000ULL check
Effect: 83 -> 96 usable slots; [TLS] 0 kept

2. Release placeholders on thread create failure
File: dlls/ntdll/unix/horizon.c
Location: __wrap_svcCreateThread (~20580)
Change: after mutex_unlock and before error log, call
        horizon_release_thread_local_placeholders(); retry create once
Effect: no std::system_error; game reaches video/play

No FEX module changes.
