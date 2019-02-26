#### 获取appid
```
	// 获取appid
	var accountInfo = wx.getAccountInfoSync() ;
	var appId = accountInfo.miniProgram.appId;
	console.log(appId);
```

