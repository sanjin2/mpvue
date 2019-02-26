#### Q:页面图片不显示问题
```
	 data() {
    return {

      img: {
        home: "../../../static/img/home_01.png", //error
      }
    };
  }
```

#### A:图片访问路径不对
```
	 data() {
    return {

      img: {
        home: "/static/img/home_01.png",  //true
      }
    };
  }
```

