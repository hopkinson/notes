# Flutter 如何用策略模式进行表单验证

在一个项目中，注册，登录，修改用户信息，下订单等功能的实现都离不开提交表单。这篇文章就阐述了如何在 Flutter 编写相对看着舒服的表单验证代码。

假设我们正在编写一个注册的页面，在点击注册按钮之前，有如下几条校验逻辑。

1. 所有表单不能为空
2. 手机号码长度为 11 位
3. 手机号码必须符合格式
4. 密码长度不能少于 6 位

## if-else 模式

我们一般可以

```dart
    if (registerForm.phone.value === '') {
            print('手机号码不能为空！')
            return false
    }
    if (registerForm.phone.length < 11) {
        print('手机号码长度为 11 位！')
        return false
    }
    if (!/^1(3|5|7|8|9)[0-9]{9}$/.test(registerForm.phoneNumber.value)) {
        print('手机号码格式不正确！')
        return false
    }
    if (registerForm.passWord.value === '') {
        print('密码不能为空！')
        return false
    }
    if (registerForm.passWord.value.length < 6) {
        print('密码长度不能少于6位！')
        return false
    }
```

编写代码，的确能够完成业务的需求，能够完成表单的验证，但是存在很多问题，比如：

1. 绑定的函数代码量比较庞大，包含了很多的 if-else 语句，看着都恶心，这些语句需要覆盖所有的校验规则。
2. 绑定的函数缺乏弹性，如果增加了一种新的校验规则，或者想要把密码的长度校验从 6 改成 8，我们都必须深入绑定的函数的内部实现，这是违反了**开放-封闭原则**的。
3. 算法的复用性差，如果程序中增加了另一个表单，这个表单也需要进行一些类似的校验，那我们很可能将这些校验逻辑复制得漫天遍野。

所谓办法总比问题多，办法是有的，比如马上要讲解的使用**策略模式**使表单验证更优雅更完美，我相信很多人很抵触设计模式，一听设计模式就觉得很遥远，觉得自己在工作中很少用到设计模式，特别是 Dart 这种灵活的语言，有的时候你已经在你的代码中使用了设计模式，只是你不知道而已。

## 策略模式

做一件事你会有很多方法，也就是所谓的策略，它的核心思想是，将做什么和谁去做相分离。所以，一个完整的策略模式要有两个类，一个是策略类，一个是环境类(主要类)，环境类接收请求，但不处理请求，它会把请求委托给策略类，让策略类去处理，而策略类的扩展是很容易的，这样，使得我们的代码易于扩展。

在表单验证的例子中，各种验证的方法组成了策略类，比如：判断是否为空的方法(如：isNoEmpty)，判断最小长度的方法(如：minLength)，判断是否为手机号的方法(isMobule)等等，他们组成了策略类，供给环境类去委托请求。

编写策略类：策略类是由一组验证方法组成的对象。

```dart

class Strategies {
  // 是否必填
  String require(value, String message) {
    return (value == null || value == '' || value == 0) ? message : '';
  }

  // 正则匹配
  String pattern(String value, RegExp reg, String message) {
    return reg.hasMatch(value) ? '' : message;
  }

  // 最小长度
  String minLength(value, int len, String message) {
    if (value is String) {
      return value.length >= len ? '' : message;
    }
    if (value is int || value is double) {
      var _number = value.toDouble();
      return _number >= len ? '' : message;
    }
    return message;
  }

  // 最大长度
  String maxLength(value, int len, String message) {
    if (value is String) {
      return value.length <= len ? '' : message;
    }
    if (value is int || value is double) {
      var _number = value.toDouble();
      return _number <= len ? '' : message;
    }
    return message;
  }
}
```

实现：

```dart
// 定义策略类
class Validate extends Strategies {
  var rules;
  var params;
  List<Function> validateList = [];

  Validate(this.rules, this.params) {
    add(rules);
  }

  start() {
    for (var index = 0; index < validateList.length; index++) {
      var message = validateList[index]();
      if (message != '') {
        return validateList[index]();
      }
    }
  }

  void add(rule) {
    var strategies = {
      'require': require,
      'pattern': pattern,
      'minLength': minLength,
      'maxLength': maxLength,
    };
    rule.forEach((keyIndex, ruleValue) {
      var _value = params[keyIndex];
      var _iterator = rules[keyIndex];
      _iterator.forEach((item) {
        item.forEach((key, value) {
          if (strategies.containsKey(key)) {
            if (key == 'require' && value == true) {
              validateList.add(() {
                return strategies[key](_value, item['message']);
              });
            } else {
              validateList.add(() {
                return strategies[key](_value, item[key], item['message']);
              });
            }
          }
        });
      });
    });
  }
}
```

使用：

```dart
class Rules {
  static RegExp isMobile = RegExp(
      r'^((13[0-9])|(14[0-9])|(15[0-9])|(16[0-9])|(17[0-9])|(18[0-9])|(19[0-9]))\d{8}$');
}
void main() {
  var params = {'phone': '1234512345123', 'password': '213', 'captcha': ''};
  var rules = {
    'phone': [
      {'require': true, 'message': '请输入手机号码'},
      {'pattern': Rules.isMobile, 'message': '请输入正确的手机号码'},
    ],
    'password': [
      {'require': true, 'message': '请输入密码'},
      {'minLength': 6, 'message': '密码长度最低6位数'},
    ],
    'captcha': [
      {'require': true, 'message': '请输入短信验证码'}
    ]
  };
  var validate = Validate(rules, params);
  var message = validate.start();
  print(message);
}

```
