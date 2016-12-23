
WKWebView 和 JS 交互

1 初始化 BKWebBridge实例 
/// 类方法
class func bridgeForWebView(_ webView : WKWebView) -> BKWebBridge {  }

/// 遍利构造方法
convenience init(webView : WKWebView) { }

2 监听 JS响应事件 ,需要先注册 JS 桥名 当 JS 代码执行时会调用 OC 方法 并传递参数
// name : JS 方法名
// handle 接受 JS 传递数据
func registerHandler(_ name : String?,handle : BKWebBridgeHandle?) 

3 调用 JS 代码 完成对页面的一些响应

// 书写完整的 JS 代码 如 : "redirect_to_order_success('付款成功')"
func runJavaScript(_ msg : String ,completionHandler : BKWebViewRunJSHandele?)


// JS方法名 + 参数 参数分为 String 或者 [:] 类型 内部会把参数转成 json 格式 

func runJavaScript(_ msg:String,data:JavaScriptResource?,completionHandler : BKWebViewRunJSHandele?) 
/// 参数放到一个 Dictionary 里边 
// redirect_to_order_success + {"return_code" : 200 ,return_message : "'成功了'"} =  redirect_to_order_success({"return_code" : 200 ,return_message : "'成功了'"})

self.bridge?.runJavaScript("redirect_to_order_success", data:["return_code" : 200,"return_message" :"'成功'"], completionHandler: { (completionHandler) in
    NJLog(completionHandler.errroMsg)
})
// 将参数放到 String 里边 如  redirect_to_order_failue + "'失败了'" = redirect_to_order_failue('失败了')
self.bridge?.runJavaScript("redirect_to_order_failue", data:"'失败了'", completionHandler: { (completionHandler) in
    NJLog(completionHandler.errroMsg)
})

/// 只要编写完整的 JS 语句 可以不使用传递参数的方式 适用于任何情形 
func runJavaScript(_ msg:String,data:JavaScriptResource?,completionHandler : BKWebViewRunJSHandele?) 
