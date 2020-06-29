---
tags:
  - frontend/react
created: 2020-03-21T12:21:20.755Z
modified: 2020-03-21T12:21:37.364Z
---

# React Fragments

Article: https://reactjs.org/docs/fragments.html

总是看到 @imkcat 写 `React.Fragment`，一直没高清楚是什么意思，今天才明白过来，原来:

    <table>
      <tr>
        <Columns />
      </tr>
    </table>

如果 `<Columns />` 是

    <React.Fragment>
      <td>Hello</td>
      <td>World</td>
    </React.Fragment>

会被编译为

    <table>
      <tr>
        <td>Hello</td>
        <td>World</td>
      </tr>
    </table>

---

`<React.Fragment></React.Fragment>` 还可以被简写为 `<></>`

佩服 @imkcat 马老哥 :face_with_thermometer:
