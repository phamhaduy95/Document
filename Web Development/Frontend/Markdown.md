remark enable transformation from markdown syntax to AST (Abstract syntax tree). User then can base on the output AST to render to different format. 

As improper use of HTML can open you up to a [cross-site scripting (XSS)](https://en.wikipedia.org/wiki/Cross-site_scripting) attacks, use of rehype can also be unsafe. Use [`rehype-sanitize`](https://github.com/rehypejs/rehype-sanitize) to make the tree safe.


Vfile is a util library that help provide virtual file service where the file is transformed without modifying original file 

lib for working with hast (html abstract syntax tree)
- unist-util-visit
- unist-util-remove-position
- unist-util-visit-parents


`\ufeff`

is the Unicode escape sequence for the character **U+FEFF** ==**ZERO WIDTH NO-BREAK SPACE**==, most commonly used as a **Byte Order Mark (BOM)**. It is an invisible character that helps software identify a text file's encoding and byte order


###### Syntax tree

[](https://github.com/unifiedjs/unified#syntax-tree)

The syntax trees used in unified are [unist](https://github.com/syntax-tree/unist) nodes. A tree represents a whole document and each [node](https://github.com/syntax-tree/unist#node) is a plain JavaScript object with a `type` field. The semantics of nodes and the format of syntax trees is defined by other projects:

- [esast](https://github.com/syntax-tree/esast) — JavaScript
- [hast](https://github.com/syntax-tree/hast) — HTML
- [mdast](https://github.com/syntax-tree/mdast) — markdown
- [nlcst](https://github.com/syntax-tree/nlcst) — natural language
- [xast](https://github.com/syntax-tree/xast) — XML

There are many utilities for working with trees listed in each aforementioned project and maintained in the [`syntax-tree`](https://github.com/syntax-tree) organization. These utilities are a level lower than unified itself and are building blocks that can be used to make plugins.


remark-rehype use 