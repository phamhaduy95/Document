`VirtualList` component would share a lots functionality with the same component from `virtuso` library.

supports `endReached` and `startReached` event
variable item size: no need for manual size declaration, accept various item with different size.

load-more support (endless scrolling)
key-board navigation
scroll to index
scroll placeholder. (if user scrolls too fast, it should display fallback UI instead)

build thin wrapper for `tansack-virtual` library


#### slot 
- itemContent
- footer:

emit
- scrolling ?
- endReached
- startReached
