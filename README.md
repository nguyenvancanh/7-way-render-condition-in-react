## 7 cách thực hiện kiểm tra điều kiện trong ứng dụng React

Cùng với sự phát triển của công nghệ, thì ngày nay, SPA (Single Page App) đang trở thành xu hướng, và dần dần thay thế cho những ứng dụng phát triển theo hướng truyền thống. Để đáp ứng được nhu cầu phát triển những ứng dụng theo kiểu SPA thì càng ngày càng có nhiều framework, nhiều ngôn ngữ lập trình ra đời để phục vụ cho mục đích trên. Các framework, hay ngôn ngữ ra đời đều phục vụ cho việc xây dựng ứng dụng một cách năng động và có tính tương tác cao. *React* cũng không nằm ngoài những ngôn nhữ "hot" nhất hiện nay. 

Trong bất kỳ ngôn ngữ lập trình nào, hay trong bất kỳ bài toán cụ thể nào thì việc kiểm tra điều kiện là vô cùng quan trọng. Gần như trong bất kỳ hoàn cảnh nào, tôi, bạn, chúng ta đều phải dùng đến câu lệnh kiểm tra điều kiện. Trong React cũng vậy, kiểm tra điều kiện được xử dụng trong các hoàn cảnh sau:

- Hiển thị dữ liệu từ 1 API

- Show/hiding elements

- Chuyển đổi chức năng ứng dụng

- Phân quyền theo cấp độ

- Xác thực và ủy quyền (Authentication vs Authorization)

Với bài viết này, mình xin đưa ra 7 phương pháp sử dụng kiểm tra điều kiện trong ứng dụng React


## Bài toán

Một bài toán đơn giản được đưa ra như sau, Hãy dựa vào trạng thái đăng nhập của user để hiển thị giao diện.

- Nếu user đã đăng nhập => hiển thị button Logout

- Nếu người dùng chưa đăng nhập => hiển thị button Login

``` React
import React, { Component } from "react";
import ReactDOM from "react-dom";
import "./styles.css";


class App extends Component {
  constructor(props) {
    super(props);
    this.state = {
      isLoggedIn: true
    };
  }
  render() {
    return (
      <div className="App">
        <h1>
          This is a Demo showing several ways to implement Conditional Rendering
          in React.
        </h1>
        <button>Login</button>
        <button>Logout</button>
      </div>
    );
  }
}


const rootElement = document.getElementById("root");
ReactDOM.render(<App />, rootElement);
```

Nếu bạn gặp bài toán như trên, thì phương án đầu tiên bạn nghĩ tới là gì ? Với mình phương án đầu tiên mình nghĩ tới là sử dụng If ... else

## 1. If ... else 

Nếu bạn là một lập trình viên, thì việc sử dụng statement if ... else chắc không còn xa lạ gì nữa đúng k? Đơn giản là với việc xử dụng If ... else, chúng ta có thể chỉ định một hành động cụ thể nếu thỏa mãn điều kiện của if, còn nếu không sẽ thực hiện câu lệnh trong else. 

#  câu lệnh kiểm tra điều kiện vào 1 function.

Trong JSX, để xử dụng câu lệnh if, thì chúng ta sẽ viết code javascript trong dấu {}. Code sẽ như sau:

```
// index.js

render() {
    let {isLoggedIn} = this.state;
    return (
      <div className="App">
        <h1>
          This is a Demo showing several ways to implement Conditional Rendering
          in React.
        </h1>
        {
          if(isLoggedIn){
            return <button>Logout</button>
          } else{
            return <button>Login</button>
          }
        }
      </div>
    );
}

```

Nhưng việc này sẽ không đảm bảo code của bạn sẽ chạy hoàn hảo. Để hiểu hơn về vấn đề này, bạn hãy tham khảo thêm link [Check](https://react-cn.github.io/react/tips/if-else-in-JSX.html)


Để giải quyết tình trạng trên, thì cách đơn giản nhất là chúng ta đưa logic của condition vào một function 

```
// index.js
...
render() {
    let {isLoggedIn} = this.state;
    const renderAuthButton = ()=>{
      if(isLoggedIn){
        return <button>Logout</button>
      } else{
        return <button>Login</button>
      }
    }
    return (
      <div className="App">
        <h1>
          This is a Demo showing several ways to implement Conditional Rendering
          in React.
        </h1>
        {renderAuthButton()}
      </div>
    );
  }
...
```

Lưu ý, chúng ta chuyển code logic vào trong hàm  renderAuthButton(), do đó cần đặt hàm này vào trong dấu {} của JSX

# Multiple return statements

Với một component, thì việc giữ cho component càng đơn giản thì việc maintainer hay phát triển về sau này sẽ càng đơn giản, Do đó, chúng ta sẽ tạo ra 1 component với tên AuthButton như sau:

```
// AuthButton.js

import React from "react";

const AuthButton = props => {
  let { isLoggedIn } = props;
  if (isLoggedIn) {
    return <button>Logout</button>;
  } else {
    return <button>Login</button>;
  }
};
export default AuthButton;
```

Component này sẽ nhận vào một biến props, và trả về một elements/components. Vì vậy khi sử dụng trong file index.js, bạn cần truyền vào gía trị như sau:

```
// index.js
...
import AuthButton from "./AuthButton";

...
  render() {
    let { isLoggedIn } = this.state;
    return (
      <div className="App">
      ...
        <AuthButton isLoggedIn={isLoggedIn} />
      </div>
    );
  }
...
```

NOTE: Hãy tránh việc code như thế này nhé

```
// index.js
...
render() {
    let { isLoggedIn } = this.state;
    if (isLoggedIn) {
      return (
        <div className="App">
          <h1>
            This is a Demo showing several ways to implement Conditional
            Rendering in React.
          </h1>
          <button>Logout</button>;
        </div>
      );
    } else {
      return (
        <div className="App">
          <h1>
            This is a Demo showing several ways to implement Conditional
            Rendering in React.
          </h1>
          <button>Login</button>
        </div>
      );
    }
  }
}
...

```

## 2. Sử dụng một biến elements

Element variable là phần mở rộng của mục ##1, Xem demo dưới đây

```
// index.js
...
render() {
    let { isLoggedIn } = this.state;
    let AuthButton;
    if (isLoggedIn) {
      AuthButton = <button>Logout</button>;
    } else {
      AuthButton = <button>Login</button>;
    }
    return (
      <div className="App">
        <h1>
          This is a Demo showing several ways to implement Conditional Rendering
          in React.
        </h1>
        {AuthButton}
      </div>
    );
  }
...
```

Code của chúng ta sẽ đơn giản hơn rất nhiều so với phương án 1 đúng k nào

## 3. Sử dụng switch

Cũng tương tự như if ... else, thì chúng ta hoàn toàn có thể sử dụng lênh switch

```
// AuthButton.js
import React from "react";

const AuthButton = props => {
  let { isLoggedIn } = props;
  switch (isLoggedIn) {
    case true:
      return <button>Logout</button>;
      break;
    case false:
      return <button>Login</button>;
      break;
    default:
      return null;
  }
};
export default AuthButton;
```

Chú ý rằng, kết quả trả về sẽ dựa vào giá trị của biến isLoggedIn. Bạn có thể dùng Switch trong trường hợp muốn trả về nhiều hơn 2 giá trị và có nhiều hơn 2 điều kiện. Luôn luôn hớ rằng phải đạt break ở cuối mỗi case nhé, với câu lệnh break sẽ giúp cho code của bạn tự kết thúc việc thực thi.


## 4. Ternary Operators

Gọi tên thì có thể bạn khó hình dung là gì, nhưng nhìn code chắc bạn sẽ nhận ra ngay

```
// index.js
...
render() {
    let { isLoggedIn } = this.state;
    return (
      <div className="App">
        <h1>
          This is a Demo showing several ways to implement Conditional Rendering
          in React.
        </h1>
        {isLoggedIn ? <button>Logout</button> : <button>Login</button>}
      </div>
    );
  }
...
```

Còn nếu bạn dùng trong component con, AuthButton thì như sau

```
// AuthButton.js
import React from "react";

const AuthButton = props => {
  let { isLoggedIn } = props;
  return isLoggedIn ? <button>Logout</button> : <button>Login</button>;
};

export default AuthButton;
```


## 5. Logical && 

```
// index.js
...
render() {
    let { isLoggedIn } = this.state;
    return (
      <div className="App">
        <h1>
          This is a Demo showing several ways to implement Conditional Rendering
          in React.
        </h1>
        {isLoggedIn && <button>Logout</button>}
      </div>
    );
  }
...
```
