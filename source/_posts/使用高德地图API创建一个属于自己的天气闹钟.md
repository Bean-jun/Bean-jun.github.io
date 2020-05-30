## 前言
本着是使用树莓派做一个个人天气预报的，通过百度也看到很多大神使用各种各样的方法来实现，本着简单、免费的想法，我去弄了个高德的开发者账号（ps:其实就是懒，哈哈哈哈哈），这个账号认证后每天有	300000次，其实也够我们自己去使用了，哈哈哈哈！后面关于文本合成语音的部分，打算接入阿里、讯飞的api的，emmm有点贵，算了，使用第三方库讲究着用吧（ps:声音确实是好僵硬）！

## 准备
1. 在这里，我们需要提前安装好requests库、pyttsx3库。   
    `pip3 install requests`   
    `pip3 install pyttsx3`
2. 进入高德开发者平台，注册账号，按照流程创建一个应用（是web服务的api），此时就可以获取到一个`key`值，这个值在后面会用到，记得保存一下。   
3. 打开高德的api使用文档，好了，开始撸代码...
## 开始撸代码
这里我们有三个任务   
- 获取当前地理位置   
- 根据当前的地理位置来获取天气情况
- 将得到的文本数据使用语音合成出来

1. 首先我们来看如何获取地理位置信息
- 这里我们可以使用web服务的api中的IP查询来获取到对应的城市名和城市的adcode编码
- 这里我们可以看下这张表
    <div class="md-atomic"><div class="table-container" style="width:100%;"><table class="md-table  " data-meta="%7B%22width%22%3A%22100%25%22%7D"><tbody><tr><th colspan="2"><p>参数名</p></th><th><p>含义</p></th><th><p>规则说明</p></th><th><p>是否必须</p></th><th><p>缺省值</p></th></tr><tr><td colspan="2" style="white-space: nowrap;"><p>key</p></td><td><p>请求服务权限标识</p></td><td><p>用户在高德地图官网申请Web服务API类型KEY</a></p></td><td><p>必填</p></td><td><p>无</p></td></tr><tr><td colspan="2" style="white-space: nowrap;"><p>ip</p></td><td><p>ip地址</p></td><td><p>需要搜索的IP地址（仅支持国内）</p><p>若用户不填写IP，则取客户http之中的请求来进行定位</p></td><td><p>可选</p></td><td><p>无</p></td></tr><tr><td colspan="2" style="white-space: nowrap;"><p>sig</p></td><td><p>签名</p></td><td><p>选择数字签名认证的付费用户必填</p></td><td><p>可选</p></td><td><p>无</p></td></tr><tr><td colspan="2" style="white-space: nowrap;"><p>output</p></td><td><p>返回格式</p></td><td><p>可选值：JSON,XML</p></td><td><p>可选</p></td><td><p>JSON</p></td></tr></tbody></table></div></div>

    ----

    <div class="md-atomic"><div class="table-container" style="width:100%;"><table class="md-table  " data-meta="%7B%22width%22%3A%22100%25%22%7D"><tbody><tr><th colspan="3"><p>名称</p></th><th><p>含义</p></th><th><p>规则说明</p></th></tr><tr><td colspan="3" style="white-space: nowrap;"><p>status</p></td><td><p>返回结果状态值</p></td><td><p>值为0或1,0表示失败；1表示成功</p></td></tr><tr><td colspan="3" style="white-space: nowrap;"><p>info</p></td><td><p>返回状态说明</p></td><td><p>返回状态说明，status为0时，info返回错误原因，否则返回“OK”。</p></td></tr><tr><td colspan="3" style="white-space: nowrap;"><p>infocode</p></td><td><p>状态码</p></td><td><p>返回状态说明,10000代表正确,详情参阅info状态表</p></td></tr><tr><td colspan="3" style="white-space: nowrap;"><p>province</p></td><td><p>省份名称</p></td><td><p>若为直辖市则显示直辖市名称；</p><p>如果在局域网 IP网段内，则返回“局域网”；</p><p>非法IP以及国外IP则返回空</p></td></tr><tr><td colspan="3" style="white-space: nowrap;"><p>city</p></td><td><p>城市名称</p></td><td><p>若为直辖市则显示直辖市名称；</p><p>如果为局域网网段内IP或者非法IP或国外IP，则返回空</p></td></tr><tr><td colspan="3" style="white-space: nowrap;"><p>adcode</p></td><td><p data-spm-anchor-id="0.0.0.i2.25166ec7hbUfrG">城市的adcode编码</p></td><td><p><br></p></td></tr><tr><td colspan="3" style="white-space: nowrap;"><p>rectangle</p></td><td data-spm-anchor-id="0.0.0.i3.25166ec7hbUfrG"><p>所在城市矩形区域范围</p></td><td><p>所在城市范围的左下右上对标对</p></td></tr></tbody></table></div></div>

    我们可以在使用时直接请求，此时可以获取到城市的adcode,那么我们直接调用返回结果即可。
    ```
    def get_citycode():
    # 获取当前位置编码
    url = 'https://restapi.amap.com/v3/ip?'
    word = None
    try:
        r = requests.get(url, params=set_params(word))
        text = r.json()
        adcode = text['adcode']
    finally:
        return adcode
    ```
2. 获取地区天气情况
- 这里我们可以使用web服务的api中的天气查询来获取到对应的城市的天气，在api使用说明中明确表示，需要传入的参数至少有adcode哦！！好在我们之前已经获取到了。 
- 开始请求数据，并处理返回的数据，这里我就取几项数据，具体可以自定义。
    ```
    def get_weather():
    # 获取当前位置天气
    url = 'https://restapi.amap.com/v3/weather/weatherInfo?'
    word = get_citycode()
    weather_dict = {}
    try:
        r = requests.get(url, params=set_params(word))
        text = r.json()
        if text['info'] == 'OK':
            weather_dict['-'] = text['lives'][0]['city']
            weather_dict['天气'] = text['lives'][0]['weather']
            weather_dict['温度'] = text['lives'][0]['temperature']
            weather_dict['空气湿度'] = text['lives'][0]['humidity']
            weather_dict['风向'] = text['lives'][0]['winddirection']
            weather_dict['风力级别'] = text['lives'][0]['windpower']
    finally:
        return weather_dict
    ```
3. 开始语音合成处理   
在这一步，需要使用到pyttsx3，查阅技术文档后，直接使用即可（ps:反正语音不是很好听，也就不做过多解释了，直接上代码）
    ```
    def read_data():
        # 阅读获取的文本
        weather = get_weather()
        engine = pyttsx3.init()
        rate = engine.getProperty('rate')
        engine.setProperty('rate', rate-10)
        engine.say(weather)
        engine.runAndWait()
    ```
4. 补充，关于代码中set_params函数部分，可以这样写就行
    ```
    def set_params(word):
    # 设置请求表单
    if word == None:
        params = {
            'key':你自己的key,
        }
    else:
        params = {
            'key':你自己的key,
            'city':word,
            # base是获取当前天气，而all可以返回预报天气，哈哈哈自己慢慢玩。
            'extensions':'base',
        }
    return params
    ```
## 结语
想不到吧，这么快就结束了！！嘿嘿嘿嘿嘿，将代码移植到树莓派上，再写一个自动化脚本，就完事啦。