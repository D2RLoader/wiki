---
title: DataTableService.getTable
description: Get a read-only view of a compiled table.
published: true
date: 2026-10-13T05:03:38.666Z
tags: 
editor: markdown
dateCreated: 2026-10-13T05:03:37.821Z
---

# DataTableService.getTable

Get a read-only view of a compiled game table.

Header: `D2RLPlugin/data_tables.h`

## Signature

```cpp
using GetTableFn = Result(__cdecl*)(
    const PluginContext* context,
    Bank bank,
    TableId tableId,
    TableView* view
) noexcept;
```

`Result`, `Bank`, `TableId`, and `TableView` are in `D2RL::DataTables`.
The function pointer is the `getTable` member of `D2RL::DataTableService`.

## Parameters

| Name | Meaning |
| --- | --- |
| `context` | The active plugin context. |
| `bank` | The table bank: Classic, LoD, or RotW. |
| `tableId` | The compiled table to read. |
| `view` | The output view. Set `structSize` to `TableViewSize` before the call. |

## Return value

Read the view only when the result is `DataTables::Result::Success`.
Do not treat a failed call as a valid empty table.

## Safety rules

- Call on the game thread.
- Do not write through the row pointer.
- Do not retain row pointers after the next table load starts.
- Check `rowSize` before casting a row to a C++ structure.
- The view's `revision` matches `DataTablesLoadedEvent`.

## Usage pattern

This function assumes that `dataTables` is an available service from the public SDK.
It counts rows without retaining the game-owned pointer.

```cpp
static auto CountLevels(const D2RL::PluginContext* context,
                       const D2RL::DataTableService* dataTables) noexcept -> uint32_t {
    D2RL::DataTables::TableView view {
        .structSize = D2RL::DataTables::TableViewSize,
        .bank = D2RL::DataTables::Bank::Lod,
    };
    const auto result = dataTables->getTable(context, D2RL::DataTables::Bank::Lod,
                                            D2RL::DataTables::TableId::Levels, &view);
    if (result != D2RL::DataTables::Result::Success) {
        return 0;
    }
    return view.rowCount;
}
```

This example returns zero on failure. A caller that needs to distinguish failure from an empty table must retain the result separately.
The complete SDK sample in `examples/data-tables` also checks the expected table revision.

## Related pages

- [DataTableService](/en/sdk/api/services/data-tables)
- [API Reference](/en/sdk/api)
