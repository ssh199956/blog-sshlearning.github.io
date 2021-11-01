---
title: GlobalKey的使用
top: true
cover: false
toc: true
mathjax: true
date: 2021-11-1 15:09:23
password:
summary: 本文整理并总结了Globalkey的学习与使用。
tags:
- Flutter
- GlobalKey
categories:
- Flutter
---

> 关注公众号【IT编程学习栈】，每日算法干货马上就来！

![](/medias/contact.jpg)

## 前言

GlobalKey 能够跨 Widget 访问状态。 在这里我们有一个 Switcher 小部件，它可以通过 changeState 改变它的状态。

```Dart

class SwitcherScreenState extends State<SwitcherScreen> {
  bool isActive = false;


  @override
  Widget build(BuildContext context) {
    return Scaffold(
      body: Center(
        child: Switch.adaptive(
            value: isActive,
            onChanged: (bool currentStatus) {
              isActive = currentStatus;
              setState(() {});
            }),
      ),
    );
  }


  changeState() {
    isActive = !isActive;
    setState(() {});
  }
}


但是我们想要在外部改变该状态，这时候就需要使用 GlobalKey。
class _ScreenState extends State<Screen> {
  final GlobalKey<SwitcherScreenState> key = GlobalKey<SwitcherScreenState>();


  @override
  Widget build(BuildContext context) {
    return Scaffold(
      body: SwitcherScreen(
        key: key,
      ),
      floatingActionButton: FloatingActionButton(onPressed: () {
        key.currentState.changeState();
      }),
    );
  }
}

```

这里我们通过定义了一个 GlobalKey 并传递给 SwitcherScreen。然后我们便可以通过这个 key 拿到它所绑定的 SwitcherState 并在外部调用 changeState 改变状态了。