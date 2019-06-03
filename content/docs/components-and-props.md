---
id: components-and-props
title: Components và Props
permalink: docs/components-and-props.html
redirect_from:
  - "docs/reusable-components.html"
  - "docs/reusable-components-zh-CN.html"
  - "docs/transferring-props.html"
  - "docs/transferring-props-it-IT.html"
  - "docs/transferring-props-ja-JP.html"
  - "docs/transferring-props-ko-KR.html"
  - "docs/transferring-props-zh-CN.html"
  - "tips/props-in-getInitialState-as-anti-pattern.html"
  - "tips/communicate-between-components.html"
prev: rendering-elements.html
next: state-and-lifecycle.html
---

Các Comp cho phép bạn chia giao diện người dùng thành các yếu tố độc lập và có thể tái sử dụng, nó cho phép bạn xem xét từng phần cô lập. Trang này cung cấp một giới thiệu về khái niệm các Components. Bạn có thể tìm thấy một [tài liệu tham khảo API thành phần chi tiết tại đây.](/docs/react-component.html).

Về mặt khái niệm, các Components giống như các hàm JavaScript. Chúng chấp nhận các đầu vào tùy ý (được gọi là "props") và trả về các phần tử React mô tả những gì sẽ xuất hiện trên màn hình.

## Function và Class Components {#function-and-class-components}

Cách đơn giản nhất để xác định một thành phần là viết JavaScript function:

```js
function Welcome(props) {
  return <h1>Hello, {props.name}</h1>;
}
```

Function này là Components React hợp lệ vì nó chấp nhận một đối số "props" duy nhất (có nghĩa là "thuộc tính") chứa dữ liệu và trả về phần tử React. Chúng tôi gọi các Components như vậy là "function components" bởi vì chúng thực sự là các JavaScript function.

Bạn cũng có thể sử dụng [es6 class](https://developer.mozilla.org/en/docs/web/javascript/reference/classes) để xác định thành phần:

```js
class Welcome extends React.Component {
  render() {
    return <h1>Hello, {this.props.name}</h1>;
  }
}
```

Hai thành phần trên tương đương với quan điểm của React.

Các class có một số tính năng bổ sung mà chúng ta sẽ thảo luận trong các [phần tiếp theo](/docs/state-and-lifecycle.html). Trong khi đó, chúng tôi sẽ sử dụng các function components cho sự ngắn gọn của chúng .

## Biện giải một Component {#rendering-a-component}

Cho đến nay, chúng ta mới chỉ bắt gặp các phần tử React đại diện cho các thẻ DOM:

```js
const element = <div />;
```

Nhưng các elements này cũng có thể đại diện cho các components do người dùng xác định:

```js
const element = <Welcome name="Tuyền" />;
```

Khi React gặp một phần tử đại diện cho một thành phần do người dùng định nghĩa, nó chuyển các thuộc tính JSX cho thành phần đó dưới dạng một đối tượng. Chúng tôi gọi đối tượng này là "props".

Ví dụ: đoạn code này hiển thị "Xin chào, Tuyền" trên trang:

```js{1,5}
function Welcome(props) {
  return <h1>Xin chào, {props.name}</h1>;
}

const element = <Welcome name="Tuyền" />;
ReactDOM.render(
  element,
  document.getElementById('root')
);
```

[](codepen://components-and-props/rendering-a-component)

Hãy tóm tắt lại những gì xảy ra trong ví dụ này:

1. Chúng tôi gọi `ReactDOM.render()` với phần tử `<Welcome name="Tuyền" />`.
2. React gọi Components `Welcome` với `{name: 'Tuyền'}` như là props.
3. Components `Welcome` của chúng ta trả về kết quả `<h1>Xin chào, Tuyền</h1>`.
4. React DOM cập nhật hiệu quả DOM để phù hợp `<h1>Xin chào, Tuyền</h1>`.

>**Lưu ý**: Luôn bắt đầu tên thành phần bằng chữ in hoa.
>
>React xử lý các Components bắt đầu bằng chữ thường dưới dạng thẻ DOM. Ví dụ, `<div />` đại diện cho thẻ div HTML, nhưng `<Welcome />` đại diện cho một Component `Welcome` tồn tại trong phạm vi hiện tại.
>
>Để tìm hiểu thêm về lý do đằng sau quy ước này, hãy đọc [chiều sâu JSX](/docs/jsx-in-depth.html#user-defined-components-must-be-capitalized).

## Thành phần Components {#composing-components}

Components có thể tham khảo các components khác trong đầu ra của chúng. Nó cho phép chúng ta sử dụng sự trừu tượng thành phần tương tự cho bất kỳ mức độ chi tiết nào. Một nút, một biểu mẫu, một hộp thoại, một màn hình: trong React, chúng thường được thể hiện bởi các components.

Ví dụ: chúng ta có thể tạo một Component `App` hiển thị Component `Welcome` nhiều lần:

```js{8-10}
function Welcome(props) {
  return <h1>Xin chào, {props.name}</h1>;
}

function App() {
  return (
    <div>
      <Welcome name="Tuyền" />
      <Welcome name="Cahal" />
      <Welcome name="Edite" />
    </div>
  );
}

ReactDOM.render(
  <App />,
  document.getElementById('root')
);
```

[](codepen://components-and-props/composing-components)

Nói chung, các ứng dụng React mới có một Components `App` duy nhất ở thư mục gốc. Mặt khác, nếu bạn tích hợp React với một ứng dụng hiện có, bạn có thể bắt đầu ở phía dưới với một Component nhỏ như `Button` và dần dần đi lên theo cách phân cấp chế độ xem.

## Extracting Components {#extracting-components}

Don't be afraid to split components into smaller components.

For example, consider this `Comment` component:

```js
function Comment(props) {
  return (
    <div className="Comment">
      <div className="UserInfo">
        <img className="Avatar"
          src={props.author.avatarUrl}
          alt={props.author.name}
        />
        <div className="UserInfo-name">
          {props.author.name}
        </div>
      </div>
      <div className="Comment-text">
        {props.text}
      </div>
      <div className="Comment-date">
        {formatDate(props.date)}
      </div>
    </div>
  );
}
```

[](codepen://components-and-props/extracting-components)

It accepts `author` (an object), `text` (a string), and `date` (a date) as props, and describes a comment on a social media website.

This component can be tricky to change because of all the nesting, and it is also hard to reuse individual parts of it. Let's extract a few components from it.

First, we will extract `Avatar`:

```js{3-6}
function Avatar(props) {
  return (
    <img className="Avatar"
      src={props.user.avatarUrl}
      alt={props.user.name}
    />
  );
}
```

The `Avatar` doesn't need to know that it is being rendered inside a `Comment`. This is why we have given its prop a more generic name: `user` rather than `author`.

We recommend naming props from the component's own point of view rather than the context in which it is being used.

We can now simplify `Comment` a tiny bit:

```js{5}
function Comment(props) {
  return (
    <div className="Comment">
      <div className="UserInfo">
        <Avatar user={props.author} />
        <div className="UserInfo-name">
          {props.author.name}
        </div>
      </div>
      <div className="Comment-text">
        {props.text}
      </div>
      <div className="Comment-date">
        {formatDate(props.date)}
      </div>
    </div>
  );
}
```

Next, we will extract a `UserInfo` component that renders an `Avatar` next to the user's name:

```js{3-8}
function UserInfo(props) {
  return (
    <div className="UserInfo">
      <Avatar user={props.user} />
      <div className="UserInfo-name">
        {props.user.name}
      </div>
    </div>
  );
}
```

This lets us simplify `Comment` even further:

```js{4}
function Comment(props) {
  return (
    <div className="Comment">
      <UserInfo user={props.author} />
      <div className="Comment-text">
        {props.text}
      </div>
      <div className="Comment-date">
        {formatDate(props.date)}
      </div>
    </div>
  );
}
```

[](codepen://components-and-props/extracting-components-continued)

Extracting components might seem like grunt work at first, but having a palette of reusable components pays off in larger apps. A good rule of thumb is that if a part of your UI is used several times (`Button`, `Panel`, `Avatar`), or is complex enough on its own (`App`, `FeedStory`, `Comment`), it is a good candidate to be a reusable component.

## Props are Read-Only {#props-are-read-only}

Whether you declare a component [as a function or a class](#function-and-class-components), it must never modify its own props. Consider this `sum` function:

```js
function sum(a, b) {
  return a + b;
}
```

Such functions are called ["pure"](https://en.wikipedia.org/wiki/Pure_function) because they do not attempt to change their inputs, and always return the same result for the same inputs.

In contrast, this function is impure because it changes its own input:

```js
function withdraw(account, amount) {
  account.total -= amount;
}
```

React is pretty flexible but it has a single strict rule:

**All React components must act like pure functions with respect to their props.**

Of course, application UIs are dynamic and change over time. In the [next section](/docs/state-and-lifecycle.html), we will introduce a new concept of "state". State allows React components to change their output over time in response to user actions, network responses, and anything else, without violating this rule.
