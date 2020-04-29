---
note:
  createdAt: 2020-04-23T05:49:10.136Z
  modifiedAt: 2020-04-23T07:20:09.775Z
  tags: [crossnote/flutter]
---

# Crossnote Flutter 调研

#crossnote/flutter

看来得重新复习下 flutter 的使用了

## libgit2

dart pub

- [git_bindings](https://github.com/GitJournal)
- [git2](https://gitlab.com/fabian.sturm/git2)

感觉可能需要我自己来做一下 ffi。可以参考一下这个官方文档: [c-interop](https://dart.dev/guides/libraries/c-interop)。

---

放弃了

突然发现 [libgit2](https://github.com/libgit2/libgit2#language-bindings) 竟然还有 [wasm-git](https://github.com/petersalomonsen/wasm-git) 以及 [nodegit](https://github.com/nodegit/nodegit) 这些 bindings，后续可以研究一下。

<!-- @timer "date":"Thu Apr 23 2020 14:15:18 GMT+0800 (China Standard Time)" -->

研究了一下，感觉自己编写 libgit2 的 ffi 难度太高，现在的思路是使用 [node-interop](https://github.com/pulyaevskiy/node-interop) 来解决 `fs`module，然后再加上 [js](https://pub.dev/packages/js) ffi 来运行 [isomorphic-git](https://github.com/isomorphic-git/isomorphic-git).

Call dart code from JavaScript
https://stackoverflow.com/questions/23104432/calling-dart-code-from-javascript

File dart
https://github.com/google/file.dart

Webview
https://pub.dev/packages/webview_flutter
