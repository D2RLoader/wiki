---
title: DataTableService
description: Read compiled game data through the public SDK.
published: true
date: 2026-10-13T05:03:39.035Z
tags: 
editor: markdown
dateCreated: 2026-10-13T05:03:36.601Z
---

# DataTableService

Read compiled game tables through the public plugin SDK.

Header: `D2RLPlugin/data_tables.h`

## Functions

- [getTable](/en/sdk/api/services/data-tables/get-table): get a view of a complete table.
- `getRow`: get a row by its row index.
- `findRowById`: find a row by its table ID value.
- `findRowByCode`: find a row by its code value.

Only `getTable` has a function page in this preview. The other entries are not links to unfinished pages.

## Related types

- `DataTables::Bank`: select Classic, LoD, or RotW data.
- `DataTables::TableId`: select a compiled table.
- `DataTables::TableView`: describe a read-only table view.
- `DataTables::RowView`: describe a read-only row view.
- `DataTables::Result`: report whether an operation succeeded.

These views do not own the game data. Their row pointers are valid only on the game thread and expire when the next table load starts.
Check the row size before interpreting a row as a C++ structure.

See [getTable](/en/sdk/api/services/data-tables/get-table) for the initial view size and a usage pattern.
