### wechat 获取用户信息
```
//网页授权获取用户基本信息，url-->生成重定向地址
app.get('/', function(req, res) {
  var url = client.getAuthorizeURL('http://' + config.domain + '/callback', '', 'snsapi_userinfo');
  res.redirect(url);
});

//授权回调
app.get('/callback', function(req, res) {
  var code = req.query.code;
  //获取基本tocken信息
  client.getAccessToken(code, function(err, result){
    if (err) console.log(err);
    var openid = result.data.openid;
    //发送文字消息测试
    _wechatApi.sendText(openid, 'Hello world', function (err,data) {
      if (err) console.log(err);
      console.log('这是一个测试消息发送'+data);
    });

    //发送模板信息测试
    // URL置空，则在发送后,点击模板消息会进入一个空白页面（ios）, 或无法点击（android）
    // {templateId} 模板id,获取模板
    var templateId = '7psOMgH0oCmQpKi2aCWrsAV0sBA-H0JsXoZuWSay5CA';
    var url = 'http://www.baidu.com';
    var name = '核磁共振波普仪';
    var data = {
      "first": {
        "value":"您预约的"+name+'预约时间快到了',
        "color":"#173177"
      },
      "name":{
        "value":name,
        "color":"#173177"
      },
      "startTime": {
        "value":"2016-06-28",
        "color":"#173177"
      },
      "endTime": {
        "value":"2016-07-29",
        "color":"#173177"
      },
      "remark":{
        "value":"请提前到指定地点使用仪器",
        "color":"#173177"
      }
    };

    //发送消息模板
    _wechatApi.sendTemplate(openid, templateId, url, data, function (err,data) {
      if (err) console.log(err);
      console.log(data);
    });

    //获取用户基本信息
    client.getUser(openid, function(err, result) {
      var userInfo = result;
      console.log('this is user info:'+userInfo);
    });
    res.redirect('/index');
  });
});
```
