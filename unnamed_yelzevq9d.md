---
note:
    id: ""
    tags: [crossnote/codemirror]
    modifiedAt: 2020-05-10T12:44:33.794Z
    createdAt: 2020-05-10T12:43:55.539Z
---
# Hint-function: Filtered hints containing HTML are escaped

[Source](https://discuss.codemirror.net/t/hint-function-filtered-hints-containing-html-are-escaped/849/3 "Permalink to Hint-function: Filtered hints containing HTML are escaped")

[dominik](https://discuss.codemirror.net/u/dominik)

[Aug '16](https://discuss.codemirror.net/t/hint-function-filtered-hints-containing-html-are-escaped/849)

Hi all!

I have a custom function for filtering hints, which filters a list of completions with a fuzzy-matching function. I want to highlight the sections of the completions that were matched against the search-term, so I marked with `<span class="highlight">`s.

For some context, here is the code-section that returns sthe result of the filtering (using the [fuzzy.js 4](https://github.com/gjuchault/fuzzyjs)-library):

    return {
      list: fuzzy.filter(search, completions, { before: '<span class="highlight">', after: '</span>' }),
      from: new CodeMirror.Pos(cursor.line, cursor.ch - currentWord.length),
      to: cursor,
    };

Unfortunately, the hint-terms get HTML-escaped, so the `<span...` is not treated as HTML, but shown as text.

Is there a way to treat the result (the `list:`-array in the code above) as HTML, not pure text?

---

No, hint text is *text*, not HTML. You can provide a custom `render` method for your hints to adjust the way they appear.

---

Thanks [@marijn](https://discuss.codemirror.net/u/marijn), this was the hint I needed!

My code looks like this now, and works perfectly:

    return {
        list: results.map(function(result) {
          return {
            string: result,
            render: function(elt, data, cur) {
              const wrapper = document.createElement('div');
              wrapper.innerHTML = result;
              elt.appendChild(wrapper);
            }
          }
        }),
        from: new CodeMirror.Pos(cursor.line, cursor.ch - currentWord.length),
        to: cursor,
      };
