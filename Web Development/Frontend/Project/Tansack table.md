road map
1. [x]  tạo 1 table API đơn giản cho phép user config table header, table data
	1. [x] design type interface cho column definition (top priority)
2. [x]  thêm tính năng row selection và multi-row selection.
	1. [ ] keyboard navigation cho table highlight
	2. [ ] support key binding for selection
	3. [x]  column visibility 

3.  thêm tính column ordering 

4. [x] client-side pagination 
5. [x] client-side sorting và filtering 
6. [x]  thêm option enable vertical virtualization cho table

Server-side table (easy but take time :( )
( manual pagination từ data fetch từ API)

pagination panel
pick page size and page index

nên dựa vào `DataTable` từ `primeVue`  library.

add aria-label support for Select with no label 

important feature

column definition với `createColumnHelper`
-  group: group nhiều data column
- access: data column chứa data field trong data model
- display:  column chứa custom data display không nằm trong data model. Ví dụ action column.


`tansack table`:
-  `headless UI` cung cấp logic như state-management, API. Developer tự build markup styling theo ý muốn của mình. 



ColumnDef
- field: The unique identifier of the column. Used to map with GridRowModel values.
- align: Align cell content.
- headerName: 
- maxWidth:
- minWidth:
- renderCell: Override the component rendered as cell for this column.
- renderHeader

su dung cell.column.id de tao slot truyen du lieu vao

performance tips
## If Possible, Avoid Cell Renderers[Copy Link](https://www.ag-grid.com/react-data-grid/scrolling-performance/#if-possible-avoid-cell-renderers)

Cell Renders result in more DOM. More DOM means more CPU processing to render, regardless of what JavaScript / Framework is used to generate the DOM.

Ask the question, do you really need the Cell Renderer?

If you are only manipulating the value rather than creating complex DOM, would a [Value Getter](https://www.ag-grid.com/react-data-grid/value-getters/) or [Value Formatter](https://www.ag-grid.com/react-data-grid/value-formatters/) achieve what you want instead? Value Getters and Value Formatters do not result in more DOM.


TreeTable



